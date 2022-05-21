---
title: "Prismaを実運用して感じたこと"
emoji: "💎"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Prisma"]
published: true
---

直近のプロジェクトでPrismaをORMとして導入と運用をしました。

運用している上で、気づいた点が色々あったのでまとめようと思います。

導入した際のメリットとデメリットで列挙していきます。コード例は雑です。

また、基本的にTypeORMとの比較で書いている部分が多いです。

## メリット
- relationが書きやすい
  - ネストする感じで書けるのでわかりやすい
    - 型安全に書ける
    ```ts
    const userAndPosts = await prisma.user.create({
        data: {
            posts: {
                create: [
                    { title: 'Prisma Day 2020' },
                    { title: 'How to write a Prisma schema' },
                ],
            },
        },
    })
    ```
## デメリット
- DBをまたぐトランザクションが貼れない
  - これが一番でかいかもしれない
  - 以下のような感じで書けるには書けるが、外のトランザクションでコミットが失敗すると中のトランザクションでロールバックできない
    ```ts
    await userPrisma.$transaction(async (prisma) => {
        await contentsPrisma.$transaction(async (prisma) => {
            ...
        }
    }
    ```
- innerJoinがやりづらい
  - whereでnotNull指定しないとinnerJoinできない
    ```ts
    const userAndPosts = await prisma.user.find({
        where: {
            NOT: [{posts: null}] 
        },
        include: {
            posts: true
        },
    })
    ```
- JOINした際のクエリがいまいち
  - JOINするごとにクエリが発行されてしまうので、ネットワークのレイテンシー次第ではボトルネックになる
      - https://www.prisma.io/docs/guides/performance-and-optimization/query-optimization-performance#solving-n1-with-include
      ```ts
      const userAndPosts = await prisma.user.find({
          include: {
              posts: true
          },
      })
      ```
      ```sql
        SELECT ... FROM user ...
        SELECT ... FROM posts ... WHERE user_id IN (...)
      ```
      - 上記だとDBアクセスが2回発生する
- インデックスヒントとかSTRAIGHT_JOINが使えない
  - これはもう生クエリ書くしかない
    - 結構使うので手軽に書きたい
- 型定義
  - 型定義ファイルを自動で作ってくれるが、交差型になってしまう
  - なので呼び出し元で抽象に依存させると型指定が面倒
    - ここはもっとうまくやる方法があるかもしれない
    ```ts
    // User & { post: Post }[]
    const userAndPosts = await prisma.user.find({
        include: {
            posts: true
        },
    })

    // 交差型で書く必要がある
    export interface IUser {
        userAndPosts(): Promise<User & { post: Post }[]>
    } 
    ```
- というかそもそもORMが遅い
  - チューニングで結局生のクエリ書くことになった

## まとめ

ほとんどデメリットになってしまいましたが、開発体験でいうとメリットの部分が結構大きいです。

小規模の運用であれば、Prismaという選択肢は結構ありな感じがします。

Prismaは開発も活発に行なわれているので今後も注目していきたいです。
