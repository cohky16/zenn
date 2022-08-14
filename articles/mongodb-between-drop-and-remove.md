---
title: "MongoDBのdropとremoveの使い分け"
emoji: "🥭"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["MongoDB"]
published: true
---

最近、職場の尊敬する先輩から「MongoDBのdropとremoveは適切に使い分けたほうがいいよ」と教わったので、復習がてらまとめたいと思います。

## 経緯

あるコレクションのデータが不要になったため、データを全削除したかったです。

今回のケースでは、コレクション自体は削除されてもバッチで再作成されるため、dropでいいだろと思い、dropを実行しました。

その後、シャーディングが有効になってないよ！とslackに通知がきたので、先輩から、そのユースケースならremoveが適してるねと指摘をもらいました。

なぜなのでしょうか。

## そもそもなぜシャーディングを有効にするのか

dropとremoveの違いの前に、シャーディングが有効になっていないことのデメリットを書いていきます。

シャーディングを一言でいうと、データを複数のサーバーに分割することです。

https://gihyo.jp/dev/serial/01/mongodb/0005#:~:text=%E8%AA%AC%E6%98%8E%E3%81%97%E3%81%BE%E3%81%99%E3%80%82-,%E3%82%B7%E3%83%A3%E3%83%BC%E3%83%89,%E3%82%92%E6%8E%A8%E5%A5%A8%E3%81%95%E3%82%8C%E3%81%A6%E3%81%84%E3%81%BE%E3%81%99%E3%80%82

そのため、シャーディングが有効になっていないと、一つのサーバーに対して負荷がかかりすぎてしまいます。

では、シャーディングをどう有効にするのかですが、シャードキーと呼ばれるキーをコレクションに設定します。

シャードキーを指定することで、このキーを元にそれぞれのサーバーに格納するデータの範囲を決めていきます。（レンジパーミッション）

https://www.mongodb.com/docs/manual/sharding/

## dropとremoveの違い

まずdropについてです。

- drop
  - 機能
    - コレクション自体を削除する
  - メリット
    - コレクションに関する情報を一挙に削除できる
  - デメリット
    - **インデックスやシャードキーがすべて失われる**

https://www.mongodb.com/docs/manual/reference/method/db.collection.drop/

実際に確認してみます。

1. シャーディングが有効になっていることを確認

```shell
mongos> db.books.getShardDistribution()

Shard shard0 at shard0/mongodb-shard0:27017
 data : 0B docs : 0 chunks : 1
 estimated data per chunk : 0B
 estimated docs per chunk : 0

Totals
 data : 0B docs : 0 chunks : 1
 Shard shard0 contains 0% data, 0% docs in cluster, avg obj size on shard : 0B
```

2. データをdropで全削除する

```shell
mongos> db.books.drop()
true
```

3. createしてもシャーディングが無効になっていることを確認

```shell
mongos> db.createCollection('books')
{
	"ok" : 1,
	"operationTime" : Timestamp(1660443597, 2),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1660443597, 2),
		"signature" : {
			"hash" : BinData(0,"noGpmOZHqGr794jV2dvXtbmb7O4="),
			"keyId" : NumberLong("7131545615913189399")
		}
	}
}
mongos> db.books.getShardDistribution()
Collection test.books is not sharded.
```

4. データベースには旧コレクションのシャーディング情報が残っている

```shell
mongos> db.printShardingStatus()
--- Sharding Status ---
  sharding version: {
  	"_id" : 1,
  	"minCompatibleVersion" : 5,
  	"currentVersion" : 6,
  	"clusterId" : ObjectId("62f856f4ad9a27d4a829cf46")
  }
  shards:
        {  "_id" : "shard0",  "host" : "shard0/mongodb-shard0:27017",  "state" : 1 }
  ...
  databases:
         ... 
        {  "_id" : "test",  "primary" : "shard0",  "partitioned" : true,  "version" : {  "uuid" : UUID("d720e030-e129-4001-84ab-1ef71dd2d7af"),  "lastMod" : 1 } }
```

removeについてはdropと同様に全ドキュメントを削除することについての記載です。

- remove
  - 機能
    - ドキュメントを削除する
  - メリット
    - インデックスやシャードキーを保持したままドキュメントを全削除できる
  - デメリット
    - 全ドキュメントに対して走査するので効率的ではない

https://www.mongodb.com/docs/manual/reference/method/db.collection.remove/

これも実際に確認します。

1. シャーディングが有効になっていることを確認

```shell
mongos> db.books.getShardDistribution()

Shard shard0 at shard0/mongodb-shard0:27017
 data : 0B docs : 0 chunks : 1
 estimated data per chunk : 0B
 estimated docs per chunk : 0

Totals
 data : 0B docs : 0 chunks : 1
 Shard shard0 contains 0% data, 0% docs in cluster, avg obj size on shard : 0B
```

2. データをremoveで全削除する

```shell
mongos> db.books.remove({})
WriteResult({ "nRemoved" : 0 })
```

3. シャーディングが有効になったままなことを確認

```shell
mongos> db.books.getShardDistribution()

Shard shard0 at shard0/mongodb-shard0:27017
 data : 0B docs : 0 chunks : 1
 estimated data per chunk : 0B
 estimated docs per chunk : 0

Totals
 data : 0B docs : 0 chunks : 1
 Shard shard0 contains 0% data, 0% docs in cluster, avg obj size on shard : 0B
```

## まとめ

実際にやってみて、どちらがいいとかはなく、ユースケースに応じて使い分けるべきだと改めて理解できました。

- remove
  - データ量が少なくて単純にデータを削除したい場合
- drop
  - データ量が多すぎて効率良くデータを削除したい場合
    - シャードキーとインデックスを再設定する必要あり

シャーディングはMongoDBにおいて重要な要素なのでしっかり理解していきたいです。
