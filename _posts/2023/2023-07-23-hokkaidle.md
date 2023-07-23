---
layout: post
title: Hokkaidle を作った
# author: takapiro_99
published: true
---

一昔前に流行った [Wordle](https://www.nytimes.com/games/wordle/index.html)、そしてそれのパロディの [Worldle](https://worldle.teuteuf.fr/)、そしてそれのパロディとして [Hokkaidle](https://hokkaidle.web.app/) を作りました。

[https://hokkaidle.web.app/](https://hokkaidle.web.app/)

初めてみたよ！という人は一つずつ遊んでみてください。

### 作り方

Worldle は MIT ライセンスで公開されている OSS だったので、それをフォークする形で作りました。（[GitHub レポジトリ](https://github.com/markgalassi/worldle)）

やることリスト

1. 北海道の市町村の形の画像（SVG など）を用意する
2. 北海道の市町村の役場の緯度経度のデータを用意する
3. それを元の Worldle の国のデータと入れ替える
4. 調整する

で進めました。

1. 北海道の市町村の形の画像（SVG など）を用意する

国土交通省が公開している「行政区域データ」で、データセットをダウンロードできました。元が GML 形式のファイルだったので、加工して SVG にしました。加工や情報収集に、以下のサイトを使わせていただきました。

- [https://nlftp.mlit.go.jp/ksj/gml/datalist/KsjTmplt-N03-v2_4.html](https://nlftp.mlit.go.jp/ksj/gml/datalist/KsjTmplt-N03-v2_4.html#!)
- [https://github.com/niiyz/JapanCityGeoJson](https://github.com/niiyz/JapanCityGeoJson)
- [https://github.com/softwaretechnik-berlin/geojson-renderer](https://github.com/softwaretechnik-berlin/geojson-renderer)

2. 北海道の市町村の役場の緯度経度のデータを用意する

以下のサイトなどなどから集めました。団体コードというコードが１で作成したデータにもあって、それで突き合わせを行うことができました。

- [https://ton2net.com/yakuba_address/](https://ton2net.com/yakuba_address/)
- [https://www.j-lis.go.jp/spd/code-address/hokkaido/cms_12214179.html](https://www.j-lis.go.jp/spd/code-address/hokkaido/cms_12214179.html)

3. それを元の Worldle の国のデータと入れ替える

置き換えるだけでした。

4. 調整する

タイトルなど、細かい箇所を直しました。

---

そして…

完成！ firebase Hosting にデプロイしています。（いつもの）

[https://hokkaidle.web.app/](https://hokkaidle.web.app/)

ぜひ遊んでみてください。

レポジトリはこちら。[https://github.com/takapiro99/hokkaidle](https://github.com/takapiro99/hokkaidle)

### 感想

ほとんどデータの前処理に使っただけでサクッと作ることができ、OSS って偉大だなあと思いました。自分からアウトプットすることも忘れないようにしていきたいです。
