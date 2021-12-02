---
layout: post
title: 冬割クーポン、どうしてもほしい
# image: /huit_logo_white.png
---

この記事は、[HUIT アドベントカレンダー 2021](https://qiita.com/advent-calendar/2021/huit) の 3 日目の記事です。

HUIT のツイッター（[@huitgroup](https://twitter.com/huitgroup)）、ぜひフォローしてね～

<br/>

---

[**さぁ！サッポロ冬割 & 冬も泊まってスマイルキャンペーン**](https://sapporo-winter2020.com/) をご存じだろうか

そう、ホテルに泊まるだけでお金がもらえる<sup>[1](#note1)</sup>というすごいキャンペーンです。

前回（2020 夏割）に引き続き、クーポンの争奪戦が始まっています。

色んなサイトから予約できますが、今回はクーポンを少しずつ放出してくれていた **るるぶ** に注目しました。

<small id="note1">1. お金はもらえない。飲食店で使えるクーポンがもらえる。</small>

---

<br/>

## 作ったもの

**4 分に一回クーポン有無を取得し、あったら通知を送ってくれるくん**

技術： `node.js` `puppetier` `crontab`

<br/>

### 作るきっかけ

今年のるるぶトラベルは、たまーに数十枚～数百枚のクーポンが放流され、それが毎回数十分で売り切れていくという仕様になっていました。

クーポンがあるときじゃないと冬割適用にならないため、みんなそのタイミングを狙って予約する訳です。初期のころは、アンテナの敏感な友達が「今クーポンでてるよー」と教えてくれてました。

おれ「でもスクレイピングで通知してくれたら便利じゃね？puppetier ちょうど興味あったし」

ということで、作り始めました。

<!-- <br/>

### サイトを観察する

たくさんカードがあって

クリックするとモーダルがでてきて、ここを開いて **初めて** あるかないかが分かります。 -->

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

規約はこんな感じ。セーフかな

<small>（[https://www.rurubu.travel/content/terms-of-use/](https://www.rurubu.travel/content/terms-of-use/)）</small>

---

#### `robots.txt`

12/01 時点で

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

テストをするのに用いたのか分かりませんが、`data-selenium` という attribute がついていたのは分かりやすくてラッキーでしたね！

<br/>

### coding time

抜粋して紹介します。

`page.evaluate()` の中身をブラウザで実行させ、スクショすべき要素の **場所** と **大きさ** を返します。あとは、完全になんとなくで、 `sleep()` つけました。

```js
const boundingClientRect = await page.evaluate(async () => {
  const sleep = (msec) => new Promise((resolve) => setTimeout(resolve, msec)); // スコープが完全に別なので、この中で sleep を定義するよ // async いらないの？って思ったけど、省略記法で返り値が Promise なので動く
  const couponCardSelector = ".CouponsGroupCard__title";
  const popupSelector = `[data-selenium="coupons-popup"]`;
  const couponAvailableSelector = `[data-selenium="coupon-card-popup-available"]`;
  const targetCardElement = Array.from(document.querySelectorAll(couponCardSelector)).filter((ele) => ele.textContent.startsWith("【さぁ！サッポロ冬割】"))[0];
  if (targetCardElement) {
    targetCardElement.parentElement.parentElement.click(); // カードをクリックし、モーダルを出現させる
    await sleep(1000);
    const popupElement = document.querySelector(popupSelector);
    const isCouponAvailable = popupElement.querySelector(couponAvailableSelector);
    const t = popupElement.getBoundingClientRect(); // この命名はテキトー
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

コード全容 ↓

[https://gist.github.com/takapiro99/070c0efe133fa91b11bdfa198c1826e3](https://gist.github.com/takapiro99/070c0efe133fa91b11bdfa198c1826e3)

<br/>

### 実際に動かす

お遊び用に借りているさくらインターネットの VPS(ubuntu18) で、`crontab` を設定しました。HUIT メンバーなら伝わる、火の鳥でおなじみの `crontab` です。

`$ crontab -e` で編集

次に以下を入れます。crontab ってデフォルトだと PATH が通されていない（なぜ？）なのでフルパスで実行させてみました。ついでにログも吐き出すようにしてみました。なんかかっこいいので。

`*/4 * * * * cd /home/takapiro/rurubu-scraper; /usr/local/bin/node ./main.js >> /home/takapiro/cron.log 2>&1`

<br/>

ネット上には、編集したらコマンドで restart しましょう！と言っている記事もありましたが、`$ man crontab` に

> After you exit from the editor, the modified crontab will be installed automatically.

とあったので、保存したらすぐ動くようになります。公式マニュアルは偉大です。

<br/>

### 結果

![image](/assets/2021/rurubu-notification.png)

動きました。やった～

クーポンが放流される時間の傾向や、なくなっていくペースも観察できました。

<br/>

これで友達と転々と冬割でホテルに泊まりに行ってみています。めでたしめでたし

<br/>

### ところで、puppeteer について

要素の取得のしかたをググると

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

今回はとりあえず動けばいいやの精神だったので、生の JavaScript で `page.evaluate()` しました。実際のブラウザと同じなので親しみやすい半面、console が見づらいなどデバッグが難しいので、ちゃんとやるなら `page.$eval()` を使ったらきれいだったと思います。

### 感想など

スクレイピングと `crontab`、知ってたけど触ったことなくて触ってみたいなと思ってたので、形にすることができて良かったです。

さくらの VPS なんて借りてないからな～って思ったみなさん、 ~~[ぜひ借りてください。](https://vps.sakura.ad.jp/)~~ 無料枠の firebase functions の定期実行で同じことができると思います（無料）。firebase ってめちゃくちゃ無料枠でかくてエンジニアとしてはありがたいですよね。大人になったらそんな素敵なものを作りたいなあ。

作業時間的には調査から完成まで 4h くらいだったかなと思います。JPHacks のタスクもあるなか、実はこっそりこれも作っていました。

<br/>

---

<br/>

明日は、 JPHacks で共に戦った @usk314 くんの記事です！楽しみですね

<br/>

おわり

<br/>
<br/>

#### 参考にしたもの

- 【2020 年度版】個人用クローラーの開発手順とその注意点 [https://qiita.com/nezuq/items/9e297d0e3468c24e8afa](https://qiita.com/nezuq/items/9e297d0e3468c24e8afa)
- puppeteer で始めるブラウザ操作の自動化 [https://www.cresco.co.jp/blog/entry/15215/](https://www.cresco.co.jp/blog/entry/15215/)
- その他各公式サイト
