---
title: "DBを少し踏み込んで理解する"
emoji: "👣"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["DB", "ACID", "N+1", "正規化"]
published: true
---

DBの基本的な操作や概要は理解したので、もう少し踏み込んで理解したいと思い調べたところ、以下3つが出てきました。

- ACID
- N+1
- 正規化

https://roadmap.sh/backend

これらは聞いたことはあったものの、細かくは理解できていなかったのでざっくりまとめてみました。

## ACID

DBのトランザクションにおける特性。以下4つの特性の頭文字を取ってACIDと呼ばれる。

- Atomicity
  - 原子性
  - 処理はすべて実行されるかすべて実行されないことを保証する特性
- Consistency
  - 一貫性
  - トランザクションによってデータに矛盾が生じないことを保証する特性
- Isolation
  - 独立性
  - トランザクションが他のトランザクションによって影響されないことを保証する特性
- Durability
  - 永続性
  - トランザクションが終了した場合データが永続的に保存されることを保証する特性

## N+1

ありがちなパフォーマンス劣化パターン。DBアクセスがN+1回発生してしまうこと。

```go
users := getUsers()
for user := range users {
  recipe := getRecipe(user.id)
}
```

対策としてはJOINしてすべてまとめて取得するか、EagerLoadingでそれぞれまとめて取得してくる。

## 正規化

DBでデータを扱いやすいように設計すること。段階ごとにDBを正規化していく。

段階としては第1から第6とボイスコッド正規化まであるが、主要な3つだけまとめる。

- 第1正規化
  - 1つのセルには1つの値しか含まない
- 第2正規化
  - 主キーに対して従属している列を切り出す
  - 部分関数従属の解消と言われる
- 第3正規化
  - 主キー以外に対して従属している列を切り出す
  - 推移的関数従属の解消と言われる