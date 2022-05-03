---
title: "ServerlessFrameworkComposeを試す"
emoji: "🔺"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Serverless Framework"]
published: true
---

ServerlessFrameworkにもDockerでいうDockerCompose的なものが来ました！

https://www.serverless.com/blog/serverless-framework-compose-multi-service-deployments

実際に私が携わっているプロジェクトでも活かせそうだと思ったので、実際に試してみました。

## 概要

ServerlessFrameworkComposeは、複数のServerlessFrameworkで記述されたサービスをまとめてデプロイできる機能です。**v3.15.0以上**で対応しています。

サービスごとにディレクトリを分けていることが多いと思いますが、今まではまとめてデプロイする場合、都度ディレクトリを移動して`serverless deploy`をする必要がありました。

それがServerlessFrameworkComposeではファイル一つ作るだけで、まとめてデプロイや削除までできちゃいます。便利。

## 使い方

以下リポジトリに手軽に試せる環境を準備しました。

https://github.com/cohky16/poc/tree/master/sls/compose

できるだけ環境を汚したくないので、コンテナ内でServerlessFrameworkを動作させるようにしてます。実際に動かしたい方は使ってみてください。

以下はこれらを作った手順です。

- Dockerfileを作成
  https://github.com/cohky16/poc/blob/master/sls/compose/Dockerfile
  - ベースイメージは適当です。
  - v3.15.0をインストールします。
- .envを作成
  - AWSの認証情報を入れる
      - AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID
      - AWS_DEFAULT_REGION=$AWS_DEFAULT_REGION
      - AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY
- docker-compose.ymlを作成
  https://github.com/cohky16/poc/blob/master/sls/compose/docker-compose.yml
  - `docker compose up -d`でコンテナを作成して動かす
  - `docker compose exec serverless /bin/bash`でコンテナに入る
- service-aとbを作成
  - `serverless create --template aws-nodejs --path service-a`
  - `serverless create --template aws-nodejs --path service-b`
  - あくまで確認なのでテンプレのまま利用する。
- serverless-compose.ymlを作成
  https://github.com/cohky16/poc/blob/master/sls/compose/serverless-compose.yml
  - pathにデプロイしたいサービスのパスを入れる
- `serverless deploy`を実行
  - `@serverless/compose`がインストールされていない場合、インストールの有無を聞かれるのでインストール
      ```shell
      ❯ serverless deploy

      Serverless Compose needs to be installed first. This is a one-time operation.
            ? Do you want to install Serverless Compose locally with "npm"? (Y/n)
      ```
   - リソースがデプロイされたことをAWS上で確認する
- `serverless remove`で削除
   - リソースが削除されたことをAWS上で確認する

以下には、serverless-compose.ymlでサービスの依存関係などさらに細かく設定する方法も書いてあります。

https://www.serverless.com/framework/docs/guides/compose

注意点として、ServerlessFrameworkComposeには今の所、以下変数しか利用することができません。

- ${sls:stage}
- ${env:xxx}

selfやopt、プラグインなどは利用できないです。

## まとめ

これだけ手軽に複数サービスの管理をできるのはすごく便利だなと感じました。

Dockerと違ってコマンドが変わらないので導入のコストが低いのもポイント高いです。

ServerlessFrameworkで複数サービスの運用に困っている方は、導入を検討してみてはいかがでしょうか。


