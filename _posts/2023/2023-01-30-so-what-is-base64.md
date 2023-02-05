---
layout: post
title: 結局 base64 って何なのか
# author: takapiro_99
# image: /huit_logo_white.png
# updated_at: 2021-10-04
published: true
---

エンジニアをしていると所々で登場する base64 について全然何も知らないなと思い、ついに調べました。

`〜〜〜==` みたいな文字列や、`eyJhbGciO〜〜` みたいな文字列に遭遇することがありますよね。そしてこれらは、なんとなく base64 感がありますよね。なぜこうなるのかを明らかにしていきましょう。

### base64 とは

[RFC4648](https://www.rfc-editor.org/rfc/rfc4648) によって定められたエンコード方式です。

> Base64 は、データを 64 種類の印字可能な英数字のみを用いて、それ以外の文字を扱うことの出来ない通信環境にてマルチバイト文字やバイナリデータを扱うためのエンコード方式である。MIME によって規定されていて、7 ビットのデータしか扱うことの出来ない電子メールにて広く利用されている。

（[https://ja.wikipedia.org/wiki/Base64](https://ja.wikipedia.org/wiki/Base64)）

とのことで、ASCII 文字しかやり取りできなかった SMTP （メール）で、例えば写真などのバイナリを送れるようにするエンコード方式のようです。基本的には \[A-Za-z+/\]の 64 種類の文字が使用されることから base64 を呼ばれているそうです。

#### base 64 のルール

１. 元データ

```js
source = "Hello World";
```

２. それをバイナリに変換する

```js
binaries = source
  .split("")
  .map((s) => s.charCodeAt().toString(2))
  .map((s) => s.padStart(8, "0"));

[
  "01001000",
  "01100101",
  "01101100",
  "01101100",
  "01101111",
  "00100000",
  "01010111",
  "01101111",
  "01110010",
  "01101100",
  "01100100",
];
```

３. 6Bit ごとにまとめる

64=2^6 なので、6bit ごとに文字にします。なお、末尾は 0 で埋めます。

```js
chunks = binaries
  .join("")
  .match(/.{1,6}/g)
  .map((s) => s.padEnd(6, "0"));

[
  "010010",
  "000110",
  "010101",
  "101100",
  "011011",
  "000110",
  "111100",
  "100000",
  "010101",
  "110110",
  "111101",
  "110010",
  "011011",
  "000110",
  "010000",
];
```

４. 変換表に従って変換する

```js
indexToLetter = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/";

encodedArray = chunks.map((c) => parseInt(c, 2)).map((c) => indexToLetter[c]);

["S", "G", "V", "s", "b", "G", "8", "g", "V", "2", "9", "y", "b", "G", "Q"];
```

５. 4 文字ずつまとめ、4 文字に足りない分は `=` を後ろに追加する。

なぜ 4 文字ずつまとめる必要があるのか。これは、例えば base64 エンコードされた文字列をくっつけた場合、デコードする際に不都合になるからなようです。たしかに。

詳しくはこちら [https://stackoverflow.com/questions/4080988/why-does-base64-encoding-require-padding-if-the-input-length-is-not-divisible-by](https://stackoverflow.com/questions/4080988/why-does-base64-encoding-require-padding-if-the-input-length-is-not-divisible-by)

```js
encoded = encodedArray
  .join("")
  .match(/.{1,4}/g)
  .map((s) => s.padEnd(4, "="))
  .join("");

// SGVsbG8gV29ybGQ= 👈完成！
```

これで完成です。デコードは、この逆になります。

<!-- ### encoder/decoder を作る JS 編

### Kotlin で編

### Rust で編 -->

### base64 っぽい文字列

冒頭に登場した base64 っぽい文字列２つですが、１つ目の `〜〜==` については、エンコード後の文字列が４の倍数の長さになるように `=` で埋めるためだということが分かりました。

では２つめの`eyJhbGciO〜〜` はなんでしょうか？答えは、JWT です。`'{"alg":"HS256","typ":"JWT", claims: ...}'` のヘッダーは大体固定のため、base64 エンコードされた JWT を base64 エンコードすると eyJhbGciOi...== みたいな見た目になりやすいということでした。

<!-- ### その他 -->

<!-- - base58 とか base32 とか 他の -->

<!-- ### まとめ -->

<!-- - base64 歴史的な経緯 -->

おわりです！
