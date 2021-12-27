---
layout: post
title: ESP32 の https 通信、遅くない？
updated_at: 2021-12-27
categories: [ESP32, ハード, プログラミング]
---

{% include from-hateblo.html %}

こんにちは。これは [LOCAL 学生部アドベントカレンダー 2020](https://adventar.org/calendars/5137) の 9 日目の記事です！

### リクエスト送るの遅くない？

この前の JPHacks で ESP32 (ESP32（と呼んでいる開発ボード）に使われている ESP32-WROOM-32 は NRND になったみたいです。今後は ESP32-WROOM-32E らしいです。)

参考: [https://tech.144lab.com/entry/esp32-wroom32-nrnd](https://tech.144lab.com/entry/esp32-wroom32-nrnd)

を使ってサーバーに情報を送信する、みたいなことをやっていたのですが、それが遅かった（一リクエスト当たり体感 3 秒くらいだった）ので、詳しく見てみようと思いました。

きっかけは 0.1s ごとにリクエスト送れば web socket 級のリアルタイム実現できるんじゃね？って遊んでた時に気づいたことでした。笑  
また、以前同じことをしたときには感じなかったし、少なくとも mqtt ではレイテンシを感じるほどじゃなかったので疑問でした。

### ESP32 と無線

ESP32 は Wi-Fi に繋いで様々なプロトコルの無線通信を行うことができます。今回はサーバーに情報を送るところで HTTP 通信の PUT を使っていました。

公式が ArduinoIDE 向けに提供しているライブラリたち ([https://github.com/espressif/arduino-esp32/tree/master/libraries](https://github.com/espressif/arduino-esp32/tree/master/libraries)) は良く整備されていている印象で、HTTP リクエストには `WiFi.h` と `HTTPClient.h` を使いました。

### 実際に測ってみる

実測をしてみます。

##### 条件

- 自宅の WiFi
- [JSON PlaceHolder](https://jsonplaceholder.typicode.com/) にリクエストを送る
- リクエストを送ってから、レスポンスを受け取るまでの時間を計測
- 10 回くらいリクエストを送り、平均を比較する。（14 回計測し、上端下端で 2 つずつ除いたものの平均を用いました）
- 実験は今(Wed Dec 9 16:43:56 JST 2020)からやります。

とします。計測項目は

1. https / PUT / リクエストボディー有り
2. https / GET
3. http / PUT / リクエストボディー有り
4. http / GET

で行きたいと思います。

### 測定の仕方

このようなものを 5 秒おきに実行しました。`millis()` は Arduino の組み込み関数(?)で、起動時からのミリ秒数を取得できます。  
シリアルモニタに出力された値を見て、記録していきました。

`http.なんとか()` で、各処理を行っています。ライブラリって便利ですね。

```c
const char* serverURL = "http(s)://jsonplaceholder.typicode.com/todos/1";

timer = millis();
HTTPClient http;
Serial.println("instanciated:" + String(millis() - timer));
http.begin(serverURL);
Serial.println("set config:" + String(millis() - timer));
// String httpRequestData = "{\"device_id\": \"jfldskajflksd\",\"weight\":\"123\"}";
// int httpResponseCode = http.PUT(httpRequestData);
int httpResponseCode = http.GET();
Serial.println("request done:" + String(millis() - timer));
String res = http.getString();
Serial.println("parsed response as string:" + String(millis() - timer));
http.end();
Serial.println("response: " + res);
Serial.print("status code: ");
Serial.println(httpResponseCode);
Serial.println("request complete:" + String(millis() - timer));
```

### 結果と考察

| 項目                   | かかった時間の平均 [ms] | curl でかかった時間の平均 [ms] |
| ---------------------- | ----------------------- | ------------------------------ |
| https / PUT / req body | 2185                    | 112                            |
| https / GET            | 1749                    | 121                            |
| http / PUT / req body  | 609                     | 460                            |
| http / GET             | 104                     | 101                            |

素で通信する分には普通に PC からリクエストを飛ばすのと遜色なしでした。それほどシンプルなプロトコルだということでしょうか。

リクエストボディーを付けると +500ms くらいかかり、特に暗号化をしようと思うと +1500ms 程度の大きな時間ロスになることが分かりました。

curl で測ってもみた ([https://qiita.com/yuya_takeyama/items/baf48a3f643e743a46b4](https://qiita.com/yuya_takeyama/items/baf48a3f643e743a46b4)) のですが、どれもそんなに大差がなかったですね。一つ気になったのは SSL/TLS なしの PUT リクエストでしたが。。。なぜでしょうね、PC が重かったとかそういうたまたまですかね。

コードではシリアル出力をしているところやレスポンスをパースしているところがありますが、それらは 数 ms だったので無視で OK です。

### 暗号化通信のために何をしているのか？

ログのモードを verbose (全部出す) に設定したところ、以下のものがでてきました（一部抜粋）
左端に出ているのは Verbose < Debug < Info のログレベルの格付けです。

```
[V][ssl_client.cpp:56] start_ssl_client(): Free internal heap before TLS 262616
[V][ssl_client.cpp:58] start_ssl_client(): Starting socket
[V][ssl_client.cpp:93] start_ssl_client(): Seeding the random number generator
[V][ssl_client.cpp:102] start_ssl_client(): Setting up the SSL/TLS structure...
[I][ssl_client.cpp:156] start_ssl_client(): WARNING: Use certificates for a more secure communication!
[V][ssl_client.cpp:180] start_ssl_client(): Setting hostname for TLS session...
[V][ssl_client.cpp:195] start_ssl_client(): Performing the SSL/TLS handshake...
[V][ssl_client.cpp:216] start_ssl_client(): Verifying peer X.509 certificate...
[V][ssl_client.cpp:225] start_ssl_client(): Certificate verified.
[V][ssl_client.cpp:240] start_ssl_client(): Free internal heap after TLS 222904
[D][HTTPClient.cpp:1025] connect():  connected to jsonplaceholder.typicode.com:443
[V][ssl_client.cpp:279] send_ssl_data(): Writing HTTP request...
[D][HTTPClient.cpp:368] disconnect(): tcp keep open for reuse
[V][ssl_client.cpp:248] stop_ssl_socket(): Cleaning SSL connection.
[V][ssl_client.cpp:248] stop_ssl_socket(): Cleaning SSL connection.
```

5 行目を見てわかる通り、こいつにはルート CA 証明書がないようです。たしかに。考えたこともなかったですね。  
（SSL/TLS については情報学 Ⅰ のテキストを見返してください）

8, 9 行目に `peer X.509 certificate` とありますが、誰の何を verify してるのかは理解できませんでした（残念）っていうか暗号化すらできているのか？ということも理解できていないので、深堀りが必要そうでした。

肝心の "どこ" で遅くなっているかについては、時間を割けず具体的には見つけられませんでしたが、またチャレンジしたいと思います！

##### 追記

うろうろしていたらドンピシャな記事を見つけました。([https://garretlab.web.fc2.com/arduino/esp32/reference/libraries/HTTPClient/HTTPClient_begin.html](https://garretlab.web.fc2.com/arduino/esp32/reference/libraries/HTTPClient/HTTPClient_begin.html))よしなにやってくれるそうです。この辺りは mbedTLS と呼ばれる技術らしいです。

[https://techtutorialsx.com/2017/11/18/esp32-arduino-https-get-request/](https://techtutorialsx.com/2017/11/18/esp32-arduino-https-get-request/)

目的のホストの証明書を取得し、ESP 側に埋め込んでおけば普通の SSL/TLS として使えるみたいです。これはまた今度やってみます。。。

### 最後に

幼稚な実験でしたが、最後まで付き合ってくれてありがとうございました。

ライブラリさえ使えればまあいいやって思ってる自分もいましたが、ちょっと掘り下げるだけで知らないことがわんさか出てきて、基礎知識の重要性を再確認できました！

おわり
