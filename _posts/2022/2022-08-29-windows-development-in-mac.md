---
layout: post
title: MacBook の localhost に Parallels(Windows 11) からアクセスする
# author: takapiro_99
# image: /huit_logo_white.png
# updated_at: 2021-10-04
---

macOS で開発を行いつつ、`<input />` の見え方やスタイルの当たり方を Windows で確認したいのでやってみた。

### やってみた

まずは Parallels Desktop を買います。[https://www.parallels.com/jp/products/desktop/buy/?new](https://www.parallels.com/jp/products/desktop/buy/?new)

次に Windows を立ち上げると、ネットワークインターフェイスが macOS 側に増えました。

```bash
$ ip a
// 省略
bridge100: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	ether fa:4d:89:57:a3:64
	inet 10.211.55.2/24 brd 10.211.55.255 bridge100
	inet6 fe80::f84d:89ff:fe57:a364/64 scopeid 0x17
	inet6 fdb2:2c26:f4e4::1/64
```

ネットに落ちていた情報からも、Windows からは `10.211.55.2` で macOS の localhost にたどり着けるようです。後で気づいたのですが、Parallels の設定から自由に変えれました。

ただし、ローカルで開発しているサーバーが localhost(127.0.0.1) しか listen していない場合がほとんどなので、こちら側もいじらないといけません。なので開発サーバーで `0.0.0.0` などを listen するように変えます。ここで、フロントエンドとバックエンドで開発サーバーが分かれている場合は同時に同じオリジンは listen できないので、それぞれ `10.211.55.2:3000` と `10.211.55.2:8000` などを listen するよう設定します。

<br/>

#### うまくいくと思いきや

開発していたアプリは認証に OAuth のサービスを利用しており、コールバック URL として `<本番URL>/callback` または `localhost:3000/callback` などだけを許可しており、`10.211.55.2:3000/callback` ではログインができず。

Windows 側から `localhost:3000` にアクセスするために、まずは `etc/hosts` の Windows 版、`C:\Windows\System32\drivers\etc` に以下の一行を追加してみるものの、なにも変わらず。

```txt
// 効かなかった1行
10.211.55.2       localhost
```

以下の記述を見て納得。localhost は絶対 `127.0.0.1` で、上書きできないようでした。

```txt
# localhost name resolution is handled within DNS itself.
#	127.0.0.1       localhost
#	::1             localhost
```

#### どうしたか

Windows の localhost を上書きできない以上、そこにプロキシを用意するしかありません。Windows に nginx をインストールし、以下の conf で動かしました。

`nginx.conf`

```txt
// 抜粋
server {
    listen       3000;

    proxy_set_header    Host    $host;
    proxy_set_header    X-Forwarded-Host      $host;
    proxy_set_header    X-Forwarded-Server    $host;
    proxy_set_header    X-Forwarded-For       $proxy_add_x_forwarded_for;

    location /api {
        proxy_pass http://10.211.55.2:8000;
    }

    location / {
        proxy_pass http://10.211.55.2:3000;
    }
}
```

すると、無事プレビューできるように :tada:

![image](/assets//2022/parallels-dev.png)

### 感想

勉強になりました。

<br/>

#### 参考

[https://qiita.com/y-w/items/887251ac283e8e22a2c5](https://qiita.com/y-w/items/887251ac283e8e22a2c5)
