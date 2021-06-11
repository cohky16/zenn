---
title: "💩なコードを書き換える~JavaScript編~"
emoji: "💩"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["JavaScript"]
published: true
---

「💩すぎる…」
昔自分の書いたコードを見直して、膝から崩れ落ちました。

そんな💩コードを今後量産しないためにも、ここに記事として残しておこうと思います。

## 単純なif文の場合は一行で

```JavaScript
const foo = true;

function doSomething() {
    console.log("🐹");
}

// 💩
if (foo) {
    doSomething();
}

// ✅
foo && doSomething();
```

実際に使うと可読性が落ちるので以下のほうが実践的かも

```JavaScript
// ⭐
if (foo) doSomething();
```

## 配列の分割代入

ES6で導入された分割代入を使う

```JavaScript
const foo = ["🍎", "🍊", "🍌"];

// 💩
const a = foo[0];
const b = foo[1];
const c = foo[2];

// ✅
const [d, e, f] = foo;
```

## mapを使う

mapとforEachの使い分けは返り値を使うかどうかで考えたほうがいい。

```js
const foo = ["🐟", "🐠", "🐡"];

// 💩
const bar = [];

foo.forEach((f) => {
    bar.push(f);
});

// ✅
const baz = foo.map((f) => {
    return f;
});
```

ここに書いたものはあくまで一例というか、今の私にとってのベストプラクティスだと思っているだけなので、改善点や修正点などあればコメントいただけるととても喜びます。