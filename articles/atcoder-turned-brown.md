---
title: "AtCoderで茶色になるまでにやったこと"
emoji: "📙"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["AtCoder", "アルゴリズム"]
published: true
---

いわゆる色変記事です。

[![Image from Gyazo](https://i.gyazo.com/34af8ce40dffdd3b3d53d2b04be68171.png)](https://gyazo.com/34af8ce40dffdd3b3d53d2b04be68171)

色々試行錯誤してやっとたどり着いた感じなので、備忘録がてらやったことを残しておこうと思います。

ちなみに私は数学的素養がほぼなくて、アルゴリズムのアの字もわからないレベルからのスタートです。

## やったこと

### AtCoderProblemsのTraining埋め

AtCoderの出題形式に慣れるためにEasyとMediumを埋めました。

MediumはたまにAGCとかで難易度が高いのが混じってる気がしたのでそこは潔く飛ばしました。

https://kenkoooo.com/atcoder/#/training

### バチャコン

古すぎると出題形式とかが変わってくるので、最新からABCのABCを埋めていきました。

茶色になるにはそれなりに速度も必要なので、早解きのトレーニングに使いました。

https://kenkoooo.com/atcoder/#/table/

### 競プロ典型 90 問

定番です。★2と★3を埋めました。茶色に行くだけならこれで十分だと思います。

私的に問題の意図を読み取る力がつく(解説で問題の意図も書いてくださっている)ので実践でかなり役立つと感じました。

https://atcoder.jp/contests/typical90

### アルゴ式

主にDPの勉強に使いました。学びたいことに関して順序立てて問題が出題されるので理解が深まりました。

https://algo-method.com/

## やってよかったこと

順番でいうと、

1. 競プロ典型 90 問
2. アルゴ式
3. バチャコン
4. AtCoderProblemsのTraining埋め

ですかね。

競プロ典型 90 問は、実践で役立つアルゴリズム力と思考力が身につくのでかなりおすすめです。難易度が幅広いので、どのレベルの方でも利用できると思います。

アルゴ式はDPや全探索など、ピンポイントで定石を勉強したい時におすすめです。

まあ正直この2つさえやってれば、緑くらいなら行けそうな気がしてます。

## ツール

コードはVSCodeで書いて、テストケースなどは`online-judge-tools`を使ってローカルで動作確認してます。

https://github.com/online-judge-tools/oj

あと、コードをgitでいい感じに管理したかったので、スクリプトを書きました。

atcoder-toolsも結構便利だなと思ったのですが、コンテスト中は通信エラーになってしまい、問題を取ってくることができなかったのでとりあえず自作でやってます。(いいのあったら使ってみたい)

https://github.com/cohky16/atcoder/blob/master/set.sh

## まとめ

茶色になるまでに色々調べて実践してみた結果、ありきたりな感じになってしまいましたが、結局は自己流ではなく、先人の方たちの真似をするのが手っ取り早いってことですかね。

今年までに緑になることが目標なので、これからも日々精進を続けていきます。
