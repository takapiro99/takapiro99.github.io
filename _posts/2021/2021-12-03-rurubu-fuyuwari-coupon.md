---
layout: post
title: 冬割クーポン、どうしてもほしい
# image: /huit_logo_white.png
published: false
---

[**さぁ！サッポロ冬割 & 冬も泊まってスマイルキャンペーン**](https://sapporo-winter2020.com/) をご存じだろうか

そう、ホテルに泊まるだけでお金がもらえるというすごいキャンペーンです。

前回（2020 夏割）に引き続き、クーポンの争奪戦が始まっています。

色んなサイトから予約できますが、今回はクーポンを少しずつ放出してくれていた **るるぶ** に注目しました。

---

<br/>

## 作ったもの

**10 分に一回クーポン有無を取得し、あったら通知を送ってくれるくん**

技術： `node.js` `puppetier` `crontab`

<br/>

### 作るきっかけ

たまーに数十枚～数百枚のクーポンが放流され、それが数十分で売り切れていくという仕様になっていました。

クーポンがあるときじゃないと冬割適用にならないため、みんなそのタイミングを狙って予約する訳です。初期のころは、アンテナの敏感な友達が「今クーポンでてるよー」と教えてくれて知っていました。

おれ「でもスクレイピングで通知してくれたら便利じゃね？puppetier ちょうど興味あったし」

ということで、作り始めました。

<br/>

### サイトを観察する

たくさんカードがあって

クリックするとモーダルがでてきて、ここを開いて **初めて** あるかないかが分かります。

<br/>

### 作る前に

スクレイピングには **大いなる責任** が伴います。

<!-- チェックすること -->

- サイトの規約
- `robots.txt`

をチェックしました。

<br/>

#### サイトの規約のそれっぽい部分

第 11 条 禁止事項 12

> 当社の承諾なく、本サービスにより得られる情報を、自己の私的利用以外の目的で複製・送信する行為、又は方法の如何を問わず第三者による利用に供する行為

規約はこんな感じ。ギリセーフかな

<small>（[https://www.rurubu.travel/content/terms-of-use/](https://www.rurubu.travel/content/terms-of-use/)）</small>

---

#### `robots.txt`

12/03 時点で

```
User-agent: *
Allow: /
Disallow: /account/
Disallow: /*/account/
Disallow: /*/book/
Disallow: /*/thankyou/
Disallow: /book/
Disallow: /thankyou/
```

今回の対象ページは `/deals` なので、OK だと判断しました。

<br/>

### 作り方

python か JavaScript で少し迷いましたが、より速く作りたかったので慣れてる JS にしました。

スクレイピングのためのライブラリは、`puppeteer` にしました。

<br/>

##### まずは、土台を整えます

`$ npm init`

`$ npm i puppeteer`

します。

<br/>

##### 次に、取得するべき DOM のセレクタを観察してみます

結果、

1. `.CouponsGroupCard__title` の要素に 「【さぁ！サッポロ冬割】…」とある要素を探す
2. その親の親の `[data-selenium="coupons-card"]` の要素をクリックする
3. `[data-selenium="coupons-popup"]` の要素（modal）が出てきたことを確認して
4. その中に `[data-selenium="coupon-card-popup-available"]` があれば available で、最後が `-expired` だと無い

ということが分かりました。

これに対して、もしクーポンがあれば `[data-selenium="coupons-popup"]` の範囲をスクショし、 discord に送信することにしました。

![image](/assets/2021/rurubu-explained.png)

<!-- <br/> -->

<br/>

テストをするのに用いたのか分かりませんが、`data-selenium` という attribute がついていたのはラッキーだと思いました。

<br/>

### coding time

[https://gist.github.com/takapiro99/070c0efe133fa91b11bdfa198c1826e3](https://gist.github.com/takapiro99/070c0efe133fa91b11bdfa198c1826e3)

<br/>

抜粋して紹介します。

`page.evaluate()` の部分は、JavaScript をブラウザで実行させ、クーポンがあった場合には要素の場所と大きさを返します。完全になんとなくで、 `sleep()` つけました。

```js
const boundingClientRect = await page.evaluate(async () => {
  const sleep = (msec) => new Promise((resolve) => setTimeout(resolve, msec)); // スコープが完全に別なので、この中で sleep を定義するよ
  const couponCardSelector = ".CouponsGroupCard__title";
  const popupSelector = `[data-selenium="coupons-popup"]`;
  const couponAvailableSelector = `[data-selenium="coupon-card-popup-available"]`;
  const targetCardElement = Array.from(
    document.querySelectorAll(couponCardSelector)
  ).filter((ele) => ele.textContent.startsWith("【さぁ！サッポロ冬割】"))[0];
  if (targetCardElement) {
    targetCardElement.parentElement.parentElement.click(); // カードをクリックし、モーダルを出現させる
    await sleep(1000);
    const popupElement = document.querySelector(popupSelector);
    const isCouponAvailable = popupElement.querySelector(
      couponAvailableSelector
    );
    const t = popupElement.getBoundingClientRect();
    return {
      result: !!isCouponAvailable,
      x: t.left,
      y: t.top,
      width: t.width,
      height: t.height,
    };
  }
  return {
    result: "no",
  };
});
```

### puppeteer の要素の取得のしかた

サーっと検索すると

- `page.$(selector)`
- `page.$$(selector)`
- `page.$eval(selector, function)`
- `page.$$eval(selector, function)`
- `page.evaluate(function)`

の 5 種があるよと出てきて、意味が分かりません。こういうときこそ公式 doc です。超分かりやすかったです。

[https://github.com/puppeteer/puppeteer/blob/main/docs/api.md#pageselector](https://github.com/puppeteer/puppeteer/blob/main/docs/api.md#pageselector)  
[https://github.com/puppeteer/puppeteer/blob/main/docs/api.md#pageevaluatepagefunction-args](https://github.com/puppeteer/puppeteer/blob/main/docs/api.md#pageevaluatepagefunction-args)

<br/>

これは僕なりの理解ですが、puppeteer は chrome(chromium?) を起動し、操っています。その中の要素の取得や操作には、その chrome 内でしか命令ができません。

`page.evaluate(function)`を使うと、渡した関数がまるまる chrome 側で実行されて、それが返した結果を得ることができます。

`page.$()` と `page.$$()` は、セレクタによって要素を取得できます。`$` だと先頭一つの要素、`$$`が二つだと複数要素です。ただし取得できるのは DOM そのものではなく、puppeteer の `ElementHandle` 型らしい。

そして `page.$eval()` はそのミックスらしい。

<br/>

今回はとりあえず動けばいいやの精神だったので、生の JavaScript で `page.evaluate()` しました。実際のブラウザで試せる半面、デバッグが難しいのでちゃんとやるなら `page.$eval()` を使ったらきれいだったと思います。

### 結果

動きました。良かったです！

クーポンが放流される時間の傾向や、なくなっていくペースも観察できました。

<br/>

### 感想

楽しかったです。

<br/>

おわり
