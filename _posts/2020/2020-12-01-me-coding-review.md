---
layout: post
title: もう12月なので一年前の自分を振り返る
updated_at: 2021-12-27
---

{% include from-hateblo.html %}

これは [HUIT アドベントカレンダー 2020](https://qiita.com/advent-calendar/2020/huit) の、2 日目の投稿です。

HUIT を知らない人は、Advent Calendar 一日目の [HUIT とは？](https://huitclub.hatenablog.com/entry/2020/12/01/060537) を読んでみてください！

HUIT は北大を中心とした情報系学生コミュニティ(サークル？)です。これからさらにワイワイできるように活動を増やそうとしているところです。興味のある人は DM してね

### 一年前、何してたっけ？

僕の一年前は web プログラミングに触れ始めて少し経った頃でした。友人とハッカソンに参加してボコボコにされたり、チーム開発に初めて挑戦したり、そんな感じです。

その頃のコードでもみて、感想を書いていくやつをやりたいと思います。

### JPHacks 2019

つい先日 [JPHacks 2020 に参加してハッカソン引退することになったはなし](https://takapitech.hatenablog.com/entry/2020/11/29/181159) をしましたが、一年前にも同じイベントに参加していて、こんなものを作っていました。

<br>

<iframe width="560" height="315" src="https://www.youtube.com/embed/fmRMO4Vgc8I" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

##### 作ったもの

光るクリスマスツリーを作りました。イルミネーションのパターンをブラウザから投稿でき、一定時間ごとに投稿されたデザインで光るというものになっています。

<figure class="figure-image figure-image-fotolife" title="みんなのツリー">
<img src="/assets/2020/dodekamin-tree.jpg" />
<figcaption>みんなのツリー</figcaption></figure>

web アプリは Django で作られていて、LED の制御は毎度おなじみ ESP32 を使っています。

[こちら](https://github.com/jphacks/SP_1906) のレポジトリでみんなで開発していました。そのときはじめての GitHub だったので、今振り返りながら見たらどんなことを思うのかを、書いていきたいと思います。

それでは行きましょう。

## よかったこと

##### 1. ちゃんとブランチを切って開発をしてた。

<img src="/assets/2020/git-graph.png" />

ちゃんとブランチを知っていました。切ってたのは一回だけですが…  
そしてよく見たら PR じゃなくて master ブランチに無理やりマージしているだけでした。惜しい

<br>

##### 2. 簡潔な DB テーブル設計

```python
class Tree(models.Model):
    date = models.DateTimeField(auto_now_add=True)
    name = models.CharField(max_length=20)
    CHOICES = (
        (1, 'クール'),
        (2, 'かわいい'),
        (3, 'おしゃれ'),
        (4, 'ばえ'),
        (5, '読める！'),
        (6, 'おもしろい'),
    )
    look = models.IntegerField(choices=CHOICES)
    data = models.TextField(max_length=5000)
    lighted = models.BooleanField(default=False)
```

木の光り方のパターンを入れるテーブル (DjangoORM のモデル定義) です。アプリ全体でこれしかなかったので正規化とかいうことは考えなくてよかったみたいですね。

<br>

##### 3. HTMLCanvas で、タッチも対応してたこと

時間なくて書くの忘れてた。ちょっとまってくれ

<br>

## 改善すべきだなって思ったところ

##### 1. 命名が意味不明すぎる

```python
ss = Tree.objects.filter(lighted=False).order_by("date").first()
treedata = Tree(lighted=True)
treedata.save()
sss = ss.data
ww = ""
n = 0
for i in range(93):
    rr = ""
    bb = ""
    ww += rr
 """ 中略 """
    n += 9
    ww += "\r"
return HttpResponse(ww)
```

`Tree` とかは分かりやすいかもです。他は意味不明です。  
コメントで何をしているか記述したり、そもそも変数名だけで何をしているか掴めるようにしておきたいですね。

(僕は自分で書いたのでわかるのですが、これはどんな光らせ方をすればいいかを教えてくれるエンドポイント(の一部)でした。)

<br>

##### 2. いたるところに `.DS_STORE` がいる

これは mac 特有のシステムファイルですが、別に git で管理する必要はありません。

最近知ったのですが、グローバルに git 管理から除外したいときには、ホームディレクトリに `~/.gitignore` を作成すれば設定できるので、mac ユーザーでお困りの方は試してみてください。ちなみに Windows もできます。

<br>

##### 3. 少なすぎるコミット、何をしたのかが分からないコミットメッセージ

コミットは全部で 14 回、 issue 0 件、PR 0 件でした。

そんなにコミットこまめにして何になるんだよって思ってました。ですが、あらゆる事故防止や後からどこで何をやったか思い出すためにも、やったことごとにコミットしたほうが良いです。コミットメッセージも統一性を持たせたり、prefix つけたりするとさらに分かりやすいらしい (上級編かも！)

<br>

##### 4. なぜか自宅サーバーにデプロイしていた

その当時ジャンク PC を買って自宅サーバー！(CentOS)とか言って遊んでたので、当たり前のようにそこにデプロイしていました。

どうやってインターネットに出ていたかって…？ サーバーに固定(local) IP を設定し、自宅の ONU でポートフォワーディングをしていました。運よく固定グローバル IP が降ってくる家だったのでできたことです。当時はイケてるなと思っていたのですが、ちょっとめんどくさいですね。

最近のハッカソンでよく見る通り Heroku や Firebase といった SaaS に簡単にデプロイできるよっていうことを教えてあげたくなりました。

<br>

### さいごに

こうやって人は成長していくのですね。それからどうやって成長してきたか気になると思うので、具体的なものを挙げてみたいと思います。

- YouTube を見てアプリ開発の手法を学んだ
- ハッカソンに参加して、分かる人にチーム開発を教えてもらった
- LT 会などに参加してエンジニアの人の話を聞いたり、発表したりした
- ツイッターでエンジニアをフォローしといた

あくまで僕の場合です、参考までに！

それではありがとうございました！

---

HUIT では初心者大歓迎です。面白いものを作りたい人やアイデアを形にしたい人がたくさん集まっています。 メンバーどうしでチームを組んでチーム開発を行うハッカソンや、アウトプットができる LT 会をたくさん企画しています。

今後も HUIT をよろしくお願いしますー！

<br>

おわり

<br>

##### 追記

このページはどこにもデプロイされてないので、整理して思い出としてデプロイしておこうと思いました。また更新します。

※追記 2021/12/27  
デプロイしました！ → [/sandbox/jphacks2019/](/sandbox/jphacks2019/)
