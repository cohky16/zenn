---
title: "Prismaでtruncateする"
emoji: "🍷"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Prisma"]
published: true
---

今までデータをリセットする際はMySQLサーバーのコンテナを作り直して再度データを入れていました。

これだと約40秒ほどかかってしまい結構時間を無駄にしていたので、コンテナを壊さずにデータを初期化するように対応しました。

これで約10秒ほどでDB内のデータをリセットすることができるようになりました。

```ts: truncate.ts
import { PrismaClient } from ".primsa/client"
 
export async function truncate(prisma: PrismaClient) {
  const allProperties = Object.keys(prisma);

  const modelNames = allProperties.filter(
    (x) => !(typeof x === "string" && (x.startsWith("$") || x.startsWith("_")))
  );

  // 外部キーを無効化
  await prisma.$queryRaw`SET FOREIGN_KEY_CHECKS=0`;

  for (const modelName of modelNames) {
    const query = `TRUNCATE TABLE ${modelName}`;
    await prisma.$queryRawUnsafe(query);
  };

  await prisma.$queryRaw`SET FOREIGN_KEY_CHECKS=1`;
}
```

## 補足

rawQueryのテンプレートリテラルには、列名とテーブル名に変数を入れることができません。

```ts
await prisma.$queryRawUnsafe`TRUNCATE TABLE ${modelName}`; // シンタックスエラーになる
```

https://www.prisma.io/docs/concepts/components/prisma-client/raw-database-access#using-variables

なので、上述のように一旦変数に格納して引数に入れるようにすれば動作させることができます。

https://github.com/prisma/prisma/issues/9765#issuecomment-1021884172


## 参考

https://zenn.dev/seya/articles/0aea06f9eb6815