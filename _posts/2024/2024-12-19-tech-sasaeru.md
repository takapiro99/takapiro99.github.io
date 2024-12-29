---
layout: post
title: このブログを支える技術
# author: takapiro_99
published: true
---

このブログは、[#力強くアウトプットする日](https://x.com/hashtag/%E5%8A%9B%E5%BC%B7%E3%81%8F%E3%82%A2%E3%82%A6%E3%83%88%E3%83%97%E3%83%83%E3%83%88%E3%81%99%E3%82%8B%E6%97%A5?src=hashtag_click&f=live) 企画で書いた記事です。

### インフラは [GitHub Pages](https://docs.github.com/ja/pages/getting-started-with-github-pages/about-github-pages)

> GitHub Pages では、既定で Jekyll を使用してサイトをビルドします。

にある通り、ほぼデフォルトの GitHub Actions に任せています。テーマは、シンプルでいいなと思った、[forever-jekyll](https://github.com/forever-jekyll/forever-jekyll)を使っています。記事は、`/_posts` 以下に Markdown ファイルで管理しています。

メンテが大変にならないように、気軽にブログを書けるように、なるべく依存が増えないようにシンプルにしたいなと思ってこうしています。

### 細かい UI で追加したライブラリ

シンプル構成だと UI の機能が限られるので、そういったところは JavaScript で補完しています。以下に追加したやつを並べていきます。

##### [glightbox](https://github.com/biati-digital/glightbox)

画像をクリックすると拡大したりカルーセル表示ができるライブラリ。例えば ↓

![image](/assets/2023/hokkaidle_image.png)

##### [disqus](https://disqus.com/)

コメントとか反応をもらえるやつ。これです！↓ 分かりやすいように枠で囲んでいます。

<div style="border: grey solid 2px;padding:10px">
<div id="disqus_thread"></div>
</div>

##### [Microlink.js](https://github.com/jroji/microlinkjs)

リンク内容を、ツイッターに貼ったときみたいにリッチに表示できるやつ。例えば ↓（Twitter のプロフ貼るとこんな長くなるのは知らなかった）

{% include linkpreview.html url='https://x.com/tomio2480' %}

このイベントを企画してくださったとみおさん。（ありがとうございます！）

---

以上が、2024 年末時点での、このブログを支える技術でした！
