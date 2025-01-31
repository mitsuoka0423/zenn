---
title: "画像分析AI(顔検証)と組み合わせよう"
---

# この章のゴール

- LINE Bot と Azure Face API を組み合わせて、顔検証 Bot を作成する

## 完成イメージ

[![Image from Gyazo](https://i.gyazo.com/d59567d7e01a7f1ec2b6e134a474bbfe.gif)](https://gyazo.com/d59567d7e01a7f1ec2b6e134a474bbfe)

## システム概要図

[![Image from Gyazo](https://i.gyazo.com/c1c3f0924ddb6a5ec989de33a1fda3ca.png)](https://gyazo.com/c1c3f0924ddb6a5ec989de33a1fda3ca)

# 話を聞くタイム

![https://i.gyazo.com/6529781dd996c64228080c383aa4a325.png](https://i.gyazo.com/6529781dd996c64228080c383aa4a325.png)

## Microsoft Azure とは？

> 公式サイトはこちら：https://azure.microsoft.com/ja-jp/

ざっくり説明します。

- Microsoft が提供しているクラウドサービスのこと
- 200 以上のサービスが提供されている
- AI・機械学習系のサービスも充実している

今日は Azure のサービスの内、AI を提供しているサービスである`Cognitive Service`の`Face API`を利用します。

## Face APIの紹介

> 公式ドキュメントはこちら：https://azure.microsoft.com/ja-jp/services/cognitive-services/face/

`Face API`には大きく分けて 3 つの機能が提供されています。

- 顔検証
- 顔検証

それぞれについて簡単に説明します。

### 顔検出

![https://i.gyazo.com/96883a0d6cec3835d1dd226f2787122f.png](https://i.gyazo.com/96883a0d6cec3835d1dd226f2787122f.png)

> 1人以上の人間の顔と各種の属性(年齢、感情、ポーズ、笑顔、顔ひげなど)を検出できます。  
> また、画像内の顔ごとに27個の特徴点が抽出されます。

### 顔検証

![https://i.gyazo.com/b95cfdb0061bed0eb926aa9340247015.png](https://i.gyazo.com/b95cfdb0061bed0eb926aa9340247015.png)

> 2つの顔が同一人物のものである可能性を検証し、信頼度スコアを取得します。

`Face API`は、複雑なプログラミングをせずに利用できる**API**として提供されています。

## ここまでのまとめ

- Azure とは、Microsoft が提供しているクラウドサービスのこと。
- Azure の AI・機械学習系のサービスの 1 つとして、**顔検証AI**が提供されている。
- 顔検証 AI は**API**として提供されているので、簡単にプロダクトに組み込める。

# 手を動かすタイム

![https://i.gyazo.com/3600fb35b96dcd212cc0d4b6f3240e74.png](https://i.gyazo.com/3600fb35b96dcd212cc0d4b6f3240e74.png)

## Face APIを使ってみよう

### 顔検証を体験する

まずは、**コーディングなし**で顔検証を体験してみましょう。
こちらの URL を開いてください。

https://azure.microsoft.com/ja-jp/services/cognitive-services/face/#demo

画像（1）と（2）に写っている人物の同一人物度を判定してくれます。

[![Image from Gyazo](https://i.gyazo.com/dfc8c50da0f9b234815463c5c75bab4d.png)](https://gyazo.com/dfc8c50da0f9b234815463c5c75bab4d)

### (演習)好きな画像で顔検証を試そう(5分くらい)

好きな人物の画像で顔検証を試してみましょう。

### (応用)APIをプレイグラウンドで使ってみる(時間がある人のみ)

https://zenn.dev/tmitsuoka0423/articles/a23bfc44bba348a0daf3

:::message
Face API のキーとエンドポイントは以下を利用してください。

- キー: `a0a4ce1df69541fbb82b5008b06bad83`
- エンドポイント: `https://test20210619.cognitiveservices.azure.com/`
:::

:::message
こちらのキーは本日のハンズオン終了後に無効化します。

- 自身でもキーとエンドポイントを発行できます。発行方法はこちらをご覧ください。(無料枠で利用できます)
- [(補足資料)Face APIのキーとエンドポイントを発行する](https://zenn.dev/tmitsuoka0423/books/939110f3e1f533d35095/viewer/03_faceapi_issue_key)
:::
## Face APIとLINE Botを組み合わせよう

### コードにFace APIのキー・エンドポイントを記入する

Gitpod のタブを開きます。

> Gitpodを一度閉じてしまった人は、[ここ](https://gitpod.io/#https://github.com/tmitsuoka0423/line-bot-azure-face-api-face-verification-handson)をクリックして再度開きましょう。  
> その際、LINE Developerコンソールから、`チャネルシークレット`と`チャネルアクセストークン`を再びコピーしてくる必要があります。

24 行目あたりにある、`faceKey`・`faceEndPoint`の行を、下記緑色の内容で上書きしてください。

```diff:index.js
// Face APIパラメータ
// 2. AIと組み合わせよう で設定します。
- const faceKey = "キー1を記入する";
- const faceEndPoint = "エンドポイントを記入する";
+ const faceKey = "a0a4ce1df69541fbb82b5008b06bad83";
+ const faceEndPoint = "https://test20210619.cognitiveservices.azure.com/";

const cognitiveServiceCredentials = new CognitiveServicesCredentials(faceKey);
const faceClient = new FaceClient(cognitiveServiceCredentials, faceEndPoint);
```

:::message
こちらのキーは本日のハンズオン終了後に無効化します。

- 自身でもキーとエンドポイントを発行できます。発行方法はこちらをご覧ください。(無料枠で利用できます)
- [(補足資料)Face APIのキーとエンドポイントを発行する](https://zenn.dev/tmitsuoka0423/books/939110f3e1f533d35095/viewer/03_faceapi_issue_key)
:::

### Expressを再起動する

ターミナルをクリックし、`Ctrl + C`を押して、Express を一度停止させます。

![https://i.gyazo.com/8357e06087a090b6d7eba35bc7b52b09.png](https://i.gyazo.com/8357e06087a090b6d7eba35bc7b52b09.png)

`^C`が出ていれば停止できています。

![https://i.gyazo.com/2b7b172dea88a6999a4e8600ca427ea4.png](https://i.gyazo.com/2b7b172dea88a6999a4e8600ca427ea4.png)

ターミナルに`node index.js`と入力して、`Enter`を押します。

![https://i.gyazo.com/0cd665b2856038452f724002cad15e24.png](https://i.gyazo.com/0cd665b2856038452f724002cad15e24.png)

Express が起動していることを確認します。

![https://i.gyazo.com/b50a862bf882e03edcf0fe5501d1e676.png](https://i.gyazo.com/b50a862bf882e03edcf0fe5501d1e676.png)
ï
以上で、顔検証 Bot の設定は終わりです。

## 顔検証Botを動かしてみよう

Bot に人の顔が写っている画像を 2 枚送信してみましょう。  
顔の同一人物度が返却されます。

[![Image from Gyazo](https://i.gyazo.com/d59567d7e01a7f1ec2b6e134a474bbfe.gif)](https://gyazo.com/d59567d7e01a7f1ec2b6e134a474bbfe)

以上で顔検証 Bot の作成はおしまいです！

# まとめ

- Azure が提供している AI・機械学習系サービスである Face API を利用するため、Azure 上でリソースを作成しました。
- LINE Bot と Face API 組み合わせて、送った 2 枚の画像の顔から同一人物かどうかを判定しました。
