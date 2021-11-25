---
layout: post
title: JPHacks 悔しかった
# author: takapiro_99
# image: /huit_logo_white.png
# updated_at: 2021-10-04
---

こんばんは

前回、[今年も JPHacks に参戦した](/2021/jphacks2021) を書きましたが、決勝大会（Award day）が終わったので感想や所感の記録です。

<br/>

結果は、賞は一つもいただけませんでした。

16 チームが参加してて、賞が 30 個くらいある（企業賞・その他色々名前のついた賞を合わせて）なかで、僕たちのチームのは賞には至らなかった、これが悔しさ倍増のポイントです。

「鳩の巣原理だから賞もらえそうだね」とかふざけて言ってました。反省しています。

※本来の鳩ノ巣原理の意味とは誤って使っています。

<br/>

### 作ったもの

予選からは、デバイスを大幅に改良しました。（首輪型 → キャップ型）

スライド：

<div style="position: relative; width: 100%; height: 0; padding-top: 56.2500%;
 padding-bottom: 48px; box-shadow: 0 2px 8px 0 rgba(63,69,81,0.16); margin-top: 1.6em; margin-bottom: 0.9em; overflow: hidden;
 border-radius: 8px; will-change: transform;">
  <iframe loading="lazy" style="position: absolute; width: 100%; height: 100%; top: 0; left: 0; border: none; padding: 0;margin: 0;"
    src="https:&#x2F;&#x2F;www.canva.com&#x2F;design&#x2F;DAEv-7H1qU4&#x2F;view?embed">
  </iframe>
</div>
<!-- <a href="https:&#x2F;&#x2F;www.canva.com&#x2F;design&#x2F;DAEv-7H1qU4&#x2F;view?utm_content=DAEv-7H1qU4&amp;utm_campaign=designshare&amp;utm_medium=embeds&amp;utm_source=link" target="_blank" rel="noopener">A_2111 チームガリガリ君</a> -->

レポ：[https://github.com/jphacks/A_2111](https://github.com/jphacks/A_2111)

<br/>

### 予選通過から決勝に向けて

予選通過から決勝まで 3 週間くらい時間が開いていたので、なにをするのかをまず話し合いました。

こんな機能あったらいいよね～とか、こういう技術触ってみたいな～とかで決めていきました。

<br/>

### 前日と当日

AirBnB で家を借りて、前日から泊まりこみで作業しました。話題のサッポロ冬割が適用になってとてもラッキーでした。クーポンですあげのスープカレーとトリトンの寿司をみんなで食べました。おいしかったです。

毎度のごとく立てた予定通り進めることができず、デバイスとソフトを連携して動かすぞー！と意気込んでいたものの結局デバイスが出来たのが当日の早朝だったり、よく考えたらその機能実装するの一晩じゃ終わらん、みたいなことも発生しました。工数見積もり、社会人になったらちゃんとできるように訓練していきたいです。

またオフラインのペアプロができて楽しかったです。後輩たちの実装力があがっていました。

![image](/assets/2021/jphacks2021-kirakira-lighting-min.png)

たくさんライティングしました。ここで映っている彼のブログを載せておきます ↓ ぜひ読んでね

[https://blog.gojiteji.com/2021/11/22/jphacks2021-ad/](https://blog.gojiteji.com/2021/11/22/jphacks2021-ad/)

<br/>

### なんで賞もらえるに至らなかったんだろうね

ハッカソンは、しばしばビジネス的な要素も評価されます。今回も、「PSF より PMF を意識した発表を」など、ビジコンか？と思わせるようなアドバイスが運営から流れていました。

JPHacks の場合多くのスポンサー企業が審査に関わるので、その色がさらに強いのかもしれません。

<br/>

もちろん賞を取りたいに越したことはないですが、今回は「みんなで圧倒的成長」「楽しんで作ってて面白いものを開発する」というテーマが僕のなか（そしてみんなの中）にありました。

プロダクトとして世の中に与える価値を考えたり、さらにそれをプレゼンで推しまくるところが甘かったかなと思います。

<br/>

それでも、褒めてくれる企業さんも多くてうれしかったです。（~~じゃあ賞くれよ~~）

<!--

「クラファンに出したら売れそう」これ去年のプロダクトでも言われたんですが、
本気で買ってくれるし売れると思ってくれているのか、お世辞なのか…

電池・充電の問題や小型化にまだまだ壁があると思っていたので、心が綺麗ではない僕には冷やかしにしか聞こえませんでした。

 -->

<br/>

### チーム開発と後輩たち

今まで参加してきたチーム開発の中でダントツで良かったのが、みんな GitHub をちゃんと使えていたということです。各自ブランチを生やして実装し、PR を出し、誰かに「レビューとマージお願いしま～す」って言ってマージしてもらう、っていう流れがちゃんとできました。

知識なくてコードレビュー分かんないっていう声もありましたが、形だけでもみんながレビューし合えるということはすごいことだと思います。

<br/>

タスク管理を issue でちゃんとできるようになったら理想かなと思いました。GitHub の issue もっと使いやすくなったらいいな。

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">Issueを立てるべきか論争になるの、理想的には気軽に立てるべきなのにGitHubの機能がいまひとつ（issueの一覧性が低い、整理できない）だから多くは管理しきれず限界を迎えるからなので、どちらかが正解ではなくてGitHubが悪い。<a href="https://t.co/PGreE3toxb">https://t.co/PGreE3toxb</a></p>&mdash; いもす (@imos) <a href="https://twitter.com/imos/status/1460784501097844741?ref_src=twsrc%5Etfw">November 17, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<br/>

### その他感想

キッチンがちゃんとあったので、料理得意なメンバーにごはん作ってもらおうと思っていたのですが、冬割のクーポンを消費しなきゃだし、備え付けの調味料が全くなかったし、徹夜で疲れてたしで実現しませんでした。

シェフのふわふわオムレツ食べたかったなあ

<br/>

### 今日の名言

<br/>

> 勝ちに不思議の勝ちあり、負けに不思議の負けなし。

<small>心形刀流・松浦静山</small>

<br/>

以上、学生最後のハッカソンレポートでした。  
おわり

---

<br/>

#### 追記

- おれたちが賞を取れなかったので、COCOA おこです
  - <blockquote class="twitter-tweet"><p lang="ja" dir="ltr">コロナ接触確認アプリ「COCOA」、バージョンアップで起動不能に　iPhone版、Android版ともに<a href="https://t.co/AIPKfrN0kY">https://t.co/AIPKfrN0kY</a> <a href="https://t.co/qogArbylFR">pic.twitter.com/qogArbylFR</a></p>&mdash; ITmedia NEWS (@itmedia_news) <a href="https://twitter.com/itmedia_news/status/1463819770055520256?ref_src=twsrc%5Etfw">November 25, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
- 後輩がブログデビューしました！→ [https://usatyo.hatenablog.com/entry/2021/11/25/224957](https://usatyo.hatenablog.com/entry/2021/11/25/224957)