---
layout: post
title: 仕事で、PDFについて学んだことをまとめる
# author: takapiro_99
published: true
---

仕事で Bill One という、請求書を受領・管理できるサービスを作っています。その中で、PDF について学んて有益だと思ったことをランダムにまとめます！

{% include linkpreview.html url='https://bill-one.com/' %}
{% include linkpreview.html url='https://speakerdeck.com/sansan33/billone-engineer' %}

## 1. PDF には 3 種類のパスワードがある

1. ユーザーパスワード

- ファイルを開くためのパスワードで、一般的に目にするやつです。

1. オーナーパスワード

- ファイルの編集や印刷を制限するためのパスワード。。このパスワードなしでも閲覧はできるので、性善説に基づいた仕様です。編集権限が無い PDF はもちろんサーバー側でも編集や結合ができないので、これのせいで一部機能に制限が出ます。

1. 添付ファイルへのパスワード

- PDF を[コンテナとして使い、ファイルを添付する](https://www.antenna.co.jp/pdf/reference/file-attachment.html)という機能があり、その添付ファイルにかけられるパスワード。

閲覧はできるけど編集や結合、抽出ができない「オーナーパスワード」がややこしく、見落としがちです。macOS 標準のプレビューで情報を閲覧することができます。

参考：アンテナハウス社の PDF ページがマジで便利

{% include linkpreview.html url='https://www.antenna.co.jp/ptl/cookbook/vol2/i02-0001.html' %}

## 2. PDF にはフォントが埋め込まれている（特に日本語を含む場合）

PDF で使われる文字のフォントは、一般的なフォントファイルである otf, ttf, Type1(圧縮された cff ファイルを含む)形式で埋め込まれます。PDF 中の文字のみを抽出したフォントのみ埋め込まれていれば良く、サブセット埋め込みとか言われてたりします。PDF の先祖である PostScript で使われていた 14 種類の標準フォントは、フォントを埋め込まなくても良いとされているみたいです。[1]

埋め込まれているフォントは、Adobe Acrobat Reader で確認するのが簡単です。文書のプロパティ＞フォント　で確認できます。例えば、以下のスクショでは、いくつかの日本語フォントが埋め込まれています。

![image](/assets/2024/embedded-fonts.png)

フォントファイルに関して、Type1 フォントは PDF の先祖の PostScript 時代の規格で、Adobe は 2023 年にそのサポートを終了しています。Type1 フォントが埋め込まれている場合は PDF ビューアーに依存した挙動になり、代替フォントによってはレイアウトが変わってしまう場合があります。

参考：
[1]: [wikipedia: PDF](https://en.wikipedia.org/wiki/PDF#Text)

埋め込まれたフォントは、抽出して見ることができます。muPDF というツールでフォントを抽出できます。

```bash
# muPDF を使う場合
$ mutool extract target.pdf
```

ttf ファイルが抽出され、これは VSCode 拡張で中身を見れたりします。（VSCode 便利！）

## 3. Web 用に最適化された PDF とは

別名、Linearized PDF。普通の PDF は、各ページにオブジェクト（画像や図形）が含まれる場合、そのオブジェクト情報はページ単位にまとまっていませが、Linearized された場合、ファイルを先頭から見ていくと、ページごとに描画できます。また、特定のページを見る場合でも、先頭にあるヒントテーブルを参照することで、アクセスが高速に行えるようになっています。

Linearize されていると、PDF ビューアーによっては、HTTP の Range ヘッダーを活用して範囲リクエストを送り、特定部分のみフェッチして通信を最適化することがあります。`206 Partial Content`を返します。

Adobe Acrobat Reader で簡単に確認できます。

![image](/assets/2024/linearized-pdf.png)

<!-- ## 4. PDF のタイムスタンプ -->

<!-- ### 他のリファレンス -->

簡単ですが、これで以上です。さらに PDF について知りたい方は、「PDF 内部構造」でググったりして色々調べてみてください。
