---
layout: post
title: LOCAL学生部総大会2020に参加してきた
updated_at: 2021-12-27
---

<br>

こんばんは、たかぴろです。

<br>

先週末に LOCAL という北海道の技術系コミュニティの学生部での、総大会という泊まりこみでもくもくしようっていう合宿のようなものに参加してきました。  
[http://students.local.or.jp/blog/entry/2020/01/23:title](http://students.local.or.jp/blog/entry/2020/01/23:title)

はい、僕は LOCAL には１か月ほど前に参加したばっかりで、初めて参加するイベントだったんですが、みんなフレンドリーで楽しくワイワイすることができました。

当日までは部長のはいばらくんと他数人しかリアルでは知らなかったのですが、いざ自己紹介を聞くとだいたい Twitter で見たことあってウケました。みんなこれからもよろしくね。

今回の企画はブログリレーということで，みんなで進捗を出してどんどんアウトプットしようという会でした。僕は、直前に参加していた北海道 LT 大会に向けて制作を進めていた **「LED をブラウザからリアルタイムで制御できるやーつ」** が、LT 大会までには自分の思う完成には至らなかったので、その機能を完成させることを目標にすることにしました。  
<br><br>

## やったこと

<br>

一日目は、榊くんに競プロを教わったり、e-sports をやったりしました。

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">榊に競プロ教わってる</p>&mdash; たかぴろ (@takapiro_99) <a href="https://twitter.com/takapiro_99/status/1218466020983111680?ref_src=twsrc%5Etfw">January 18, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">榊とe-sportsした <a href="https://t.co/TKq42yc9Cf">pic.twitter.com/TKq42yc9Cf</a></p>&mdash; たかぴろ (@takapiro_99) <a href="https://twitter.com/takapiro_99/status/1218468921554006016?ref_src=twsrc%5Etfw">January 18, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

宿に移動してからは、たしかすぐ AtCoder の企業コンがはじまり、100-200-...という感じの点数だった気がするけど 100 しかできずにレート下がった。榊は時間いっぱいがんばっててすごいなって思った。緑以上くらいなりたいなーとぼんやり思うけど思ってるだけじゃなんも変わらんな(あたりまえ)

<br>
その後は食堂に集まって夜のもくもく()がスタートしたけど、もくもくだけじゃなくて質の高い情報交換会になったなっていう印象でした。参加者の中には17の代(高２！)の人たちがいて、その人たちがAWS～Azure～とかVue～とか言っててマジで天才だなーって。勉強することももちろん大切だけど能動的にこう技術に取り組めるっていいですよね。刺激になりました。おれは２時くらいに寝たっけ？

<br><br>

## やったこと(二日目)

LED ぴかぴかーの実装をメインに取り組みました。具体的にいうと、ESP32 でぴかぴか情報を受け取って、それを LED に流していくところのコーディングです。時間に追われながらなんとか動かすことができました。

ソースはこちらにあります。(ぐちゃぐちゃだけど)(あとでぜったい整理する)(というか鯖 IP バレバレだけどどうすればいいんだ???)

[https://github.com/takapiro99/iotsword_2](https://github.com/takapiro99/iotsword_2)

<br>
<br>

昼を挟んで進捗報告 LT 会をしました。有志で発表していったけどあれ参加者みんなの LT 聞きたかったな、あ、こういうことやってたのかーみたいなのが後からあった。今度はリクエストしてみよう。  
<br>

なゆたの「ちゃんと勉強もがんばる」っていうのがおれにはグサッと刺さりました。勉強もがんばります。そんなこんなで地方勢のバスがあるので超健全な時間に解散。

<br><br>

### できたもの

<br>
<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">動画！<br>ついでに同時閲覧数と謎のチャット機能つけた、<a href="https://t.co/uRtiigZ1rA">https://t.co/uRtiigZ1rA</a>の勉強になりました <a href="https://t.co/yOud1bpGcR">pic.twitter.com/yOud1bpGcR</a></p>&mdash; たかぴろ (@takapiro_99) <a href="https://twitter.com/takapiro_99/status/1218924116331286528?ref_src=twsrc%5Etfw">January 19, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
<br>

### 感想

- イベント駆動開発最高だね
- すごい成長になったしアウトプットもできた気がする
- 競プロつよいひとほんとすごい
- 思いのほか Django 人気だったね(僕含め３組が Djangoer(?)だった)
- 高専勢キーボード打つのはやすぎ。。。まぢ寿司打しょ。。。

<br><br>

## 最後に

たくさんの支援、ほんとうにありがとうございました。会場を提供してくれたビットスターさん、数々のノベルティ＆スポンサーであるさくらインターネットとみたにさん、そして LOCAL のみなさんにはとても感謝です。

<br><br>
ブログを書いたのでぼくの学生部総大会はこれでおわりです。

## <br><br>

#### 近況

このイベントの後、Janog に参加したりその後に DNS 温泉に行ったりと忙しくこれを書くのが遅くなってしまいました。早いうちにそいつらについても書きます。近況ですが Django+React で RestAPI+SPA の勉強をしています。おもしろいからです。知らないことを知っていくのはおもしろいです。でも学校の勉強も大事でそれらもしっかりと取り組んでいいかなければなあと感じています。それとイベント行ったよ記事だけじゃなくてガンガンコードが出てくるような記事も書いてみたいなと思ってきました。今度やってみます。ここまで読んでくれてありがとうございました。
