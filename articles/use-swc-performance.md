---
title: "NestJSのビルドをswcで高速化した話"
emoji: "🕙"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["swc", "NestJS", "TypeScript", "ts-node"]
published: true
---

今携わっているプロジェクトでビルドが壊滅的に遅かったため、改善をしました。

ベンチマークは以下のような感じです。

|before|after|
|---|---|
|70.316s|4.831s

約1/14になりました。

## 開発環境

NestJSで開発しており、デフォルトはts-nodeでトランスパイルをするようになっているため、かなりパフォーマンスが悪かったです。(型チェックの影響)

そこで色々なコンパイラをさがしていたところ、swcがもっとも良さそう(rust製)だったため、こちらでトランスパイルするように対応しました。

## 対応方法

- ライブラリの導入
- swcrcファイルの作成
- トランスパイル

### ライブラリの導入

yarnにもあります。

https://swc.rs/docs/getting-started

```shell
yarn add -D @swc/cli @swc/core
```

### swcrcファイルの作成

設定ファイルを作成します。

https://swc.rs/docs/configuration/swcrc

NestJSの場合はデコレーター周りの設定をする必要があります。

```json:.swcrc
{
  "jsc": {
    "parser": {
      "syntax": "typescript",
      "decorators": true,
    },
    "target": "es2020",
    "keepClassNames": true,
    "transform": {
      "legacyDecorator": true,
      "decoratorMetadata": true,
    }
  },
  "module": {
    "type": "commonjs",
    "noInterop": true
  }
}
```

### トランスパイル

sオプションでソースマップを有効化

https://swc.rs/docs/usage/cli

```json:package.json
"build": "npx swc ${トランスパイル対象のパス} -s -d ${出力先のパス}"
```

## まとめ

これだけの設定でここまでビルドを高速化できるのは驚きました。

型チェックをしないとはいえど、最近のIDEで型チェックしないものはほとんどないと思うので、型チェックをしないというのはさほど問題ではないと思いました。