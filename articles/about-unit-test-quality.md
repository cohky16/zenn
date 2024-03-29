---
title: "単体テストに関する知見"
emoji: "👷"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["単体テスト", "品質向上"]
published: true
---

最近プロダクトの品質を上げるための第一歩として、テストカバレッジを上げよう施策に携わっています。

しかし、ただカバレッジを上げるだけでは品質向上に繋げることはできず、むしろ技術的負債を生みかねません。

そこで、単体テストについて改めて調べてみることにしました。

## 単体テストとは

そもそも単体テストとは、**システムを構成する小さなユニットが個々の機能の役割を正しく果たしているかを検証するテスト**になります。

小さなユニットとは、関数だけではなく、クラスやモジュール全体になることもあります。

そのため、関数を個々にテストするものが単体テストということではありません。

### メリット

#### 大胆なリファクタリングを気軽に行うことができる

私的にですが、単体テストを書く上でここが一番大きなメリットかなと思っています。

テストを書いていないコードをリファクタリングすることへの恐怖は、何とも形容し難いものがあります。

この恐怖を味わってしまったが最後、そのコードがリファクタリングされることは永久にありません。笑

単体テストがしっかり整備されていれば、ユニットの挙動を大きく変えるような変更が加わった場合、テストが失敗するようになっているはずです。

そのため、テストが成功している限り、デグレを恐れることなく、大胆にリファクタリングを行うことができます。

#### コードの振る舞いを理解することができる

単体テストを書く際、if(もしこの場合)then(こうする)のようなケースで表現されることが多いと思います。

そう考えてみると、単体テストは仕様書と同等の役割を果たしていると言えます。

そのため、単体テストを読んだり書いたりすることで、テスト対象のユニットがどのようなケースでどのような役割を果たすのかを理解することができます。

よく言われるTDDはこれを利用して、先に仕様書を落とし込む形で単体テストを書き、その後に単体テストが成功する形で実装をするといったことをしています。

### デメリット

#### 書くコストがかかる

当然ですが、テストを書くのと書かないとでは、その場のコストは書かない方が低いです。

しかし、テストを書くことでコードの保守性があがるので、中長期的に考えると書かない方が全体のコストとしては高くなります。

詳しい話は以下のnoteにわかりやすくまとまっているので気になる方は是非。

https://note.com/cyberz_cto/n/n26f535d6c575

#### 品質の低いテストは技術的負債になる

品質の低いテストの一例として、

- 不安定
- 検証が不足している
- 可読性が低い

テストはすべて成功することが前提なので、基本的にコードはテストに追従する形になります。（テスト成功もコードありきなので追従という表現は適切でないかも？）

そのため、品質の低いテストを書いてしまうと、コードを修正するたびにテストも修正しなければならなくなるため、技術的な負債になってしまいます。

## テスト手法

単体テストについては理解できたので、これからはどうテストを書いていくのかについてをまとめていきます。

### 計画

実装に入る前に、テストを作成するためのコストを把握することが重要です。

「対象のコードがテストを書きやすいかどうか」を判断します。

そこでよく使われるのがテスト容易性です。

#### テスト容易性

別名、テスタビリティとも言われます。

このテスト容易性については一言でいうのは難しいので、一般的に言われているテスト容易性を構成する要素を列挙していきたいと思います。

- 実行円滑性
  - テストが実行しやすいか
- 観測容易性
  - テスト対象が観測しやすいか
- 制御容易性
  - テスト対象が操作しやすいか
- 分解容易性
  - テスト対象が分離、分割しやすいか
- 単純性
  - テスト対象が単純か
- 安定性
  - テスト対象が安定しているか
- 理解容易性
  - テスト対象が理解しやすいか

https://thinkit.co.jp/article/14136

これらのテスト容易性が低いとテストが書きづらく、高いとテストが書きやすくなります。

テスト容易性を高くするために一番重要なポイントとして、「結合度を低く、凝集度を高くする」ことです。

一番重要なポイントとしたのは、結合度を低く、凝集度を高くすることによって、テスト容易性を高くするためのその他のポイントである、

- 拡張性
- 条件の制限
- 品質のバランス

これらすべてを満たせると思ったからです。（あくまで主観）

#### 結合度と凝集度

結合度とは、モジュール間の依存性の程度を評価します。

結合度には以下のようにレベルがあります。↑にいくと結合度が高く、↓にいくと低くなります。

また、結合度が低いと、可読性と保守性が上がります。

- 内部結合
  - 特定のモジュールが別のモジュールの内部動作に依存している状態
  ```js
  function getUserFullNameById(userId) {
    const user = await userRepository.findOne(userId);
    return user.getFirstName() + user.getLastName(); // ミドルネームなどが追加された時に対応できない
   }
  ```
- 共通結合
  - 複数のモジュールが同じグローバルデータにアクセスできる状態
  ```js
  let data;

  function updateA() {
    data.value = 'A';
  }

  function updateB() {
    data.value = 'B';
  }
  ```
- 外部結合
  - 標準化されたインターフェースをもつグローバルな状態を共有している状態
  ```js
  function function1() {
    Api.getData();
  }

  function function2() {
    Api.update(Data());
  }
  ```
- 制御結合
  - 特定のモジュールに情報を渡して別のモジュールの流れを制御している状態
  ```js
  function function1() {
    function2(true);
  }

  function function2(flag) {
    if (flag) {
      console.log('A');
    } else {
      console.log('B');
    }
  }
  ```
- スタンプ結合
  - 構造体やクラス等の受け渡しで結合されている状態
  ```js
  function function1() {
    function2(User(name = 'hoge'));
  }
  ```
- データ結合
  - 単純な引数のやりとりをしている状態
  ```js
  function function1() {
    function2(123, 'abc');
  }
  ```
- メッセージ結合
  - 引数のないやりとりをしている状態
  ```js
  function function1() {
    function2();
  }
  ```

凝集度とは、モジュール内の協調度を評価します。

凝集度にも結合度と同様にレベルがあります。↑にいくと凝集度が低く、↓にいくと高くなります。

- 偶発的凝集
  - 無作為に集められている状態
  ```js
  function main() {
    const data = getData(); // データを取得
    console.log('hoge'); // 出力
    calcPrimeNumber(10); // 素数の計算
  }
  ```
- 論理的凝集
  - 論理的に似ているものが集められている状態
    - 凝集度でいうと制御結合にあたる
  ```js
  function sample(isA) {
    if (isA) {
      sampleA();
    } else {
      sampleB();
    }
  }
  ```
- 時間的凝集
  - 時間的に近く動作するものが集められている状態
    - 中身の実行順序を入れ替えても動作する特徴を持つ
  ```js
  function init() {
    initConfig(); // 設定の初期化
    initLogger(); // ロガーの初期化
    initDB(); // DBの初期化
  }
  ```
- 手順的凝集
  - 順番に実行する必要があるものが集められている状態
  ```js
  function outputFile(file) {
    checkPermission(); // 権限の確認
    writeFile(file); // ファイル出力
  }
  ```
- 通信的凝集
  - 同じデータを扱う部分を集めた状態
  ```js
  function changeAll(data) {
    changeA(data);
    changeB(data);
    changeC(data);
  }
  ```
- 逐次的凝集
  - ある部分の出力が別の部分の入力となるような部分を集めた状態
  ```js
  function sample() {
    const file = getFile(); // ファイルを取得
    const transformed = transform(file); // ファイルを変換
    saveFile(transformed); // ファイルを保存
  }
  ```
- 機能的凝集
  - 単一の定義されたタスクを実現している状態
  ```js
  // 線分の長さを計算する
  function calcLength(x1, y1, x2, y2) {
    return ((x2 - x1) ** 2 + (y2 - y1) ** 2) ** 0.5;
  }
  ```

結合度と凝集度は関連性が強く、結合度が低くなれば凝集度が高くなり、結合度が高くなると凝集度も低くなります。

テスト容易性、結合度と凝集度を意識して、テストがしやすいものから先に実装して、しづらいものは後回しにするなど、計画的にテストを進めていくことが重要です。

### 指標

テストを書く際に何かしらの指標がないと、どこまで書いていいのかがわからなくなると思います。

そこで、一定の基準としてカバレッジ（網羅率）を指標としているところが多いです。

#### カバレッジとは

所定の網羅条件が、テストで実行された部分の割合を表す指標です。

網羅条件はC0,C1,C2の一般的には3種類あります。以下の関数を使って説明していきます。

```js
function hoge(arg1, arg2) {
  if (arg1 > 2) {
    // 1
  } else {
    // 2
  }

  if (arg1 % 2 == 0 || input % 2 != 0) {
    // 3
  } else {
    // 4
  }
}
```

- C0
  - 別名
    - 命令網羅
  - 詳細
    - すべての命令を実行することで100%になります。
  - 例
    - 1,2,3,4を通るように書けば100%になります。
- C1
  - 別名
    - 分岐網羅
  - 詳細
    - すべての分岐の組み合わせを実行することで100％になります。
  - 例
    - 1&3,1&4,2&3,2&4を通るように書けば100%になります。
- C2
  - 別名
    - 条件網羅
  - 詳細
    - すべての条件結果を実行することで100%になります。
  - 例
    - 1&3,1&4,2&3,2&4の`arg1 % 2 == 0 || input % 2 != 0`のtrue,falseそれぞれのパターンを書けば100%になります。

ただ、カバレッジはあくまで指標です。

遵守するために不自然なコードを追加したり、品質を劣化させるようなことをすると技術的な負債につながるので注意しましょう。

### 実装

ここでやっと実装の話になってきます。

テストケースを作成する方法は2パターンあります。

#### ホワイトボックステスト

モジュールの内部構造が設計や仕様書通りに作成され、正しい動作をしているかどうかを検証します。

内部構造のテストになるので、カバレッジを指標にして実装されることが多いです。

#### ブラックボックステスト

モジュールへの入力に対して正しい出力が得られるかどうかを検証します。

重要になるのが、ブラックボックステストの際、内部構造は意識しません。

あくまでそのモジュールが期待する振る舞いをしているかどうかを検証します。

同値分割や境界値テストによって、実装されることが多いです。

#### テストダブル

テストしたいモジュールが外部APIなどに依存している場合、挙動が制御できないため不安定なテストになってしまいます。

そこで、スタブやモックなどのテストダブルを使って外部APIを差し替えることで安定したテストにすることができます。

しかしここで重要になるのが、なんでもかんでもテストダブルにすることがいいわけではありません。

例えば以下の場合は、そのまま依存モジュールを使用した方がいいです。

- スタブすることで記述量が爆発的に増える
- 依存モジュールが密結合になっており、差し替えに多大なコストがかかる

## まとめ

単体テストについて色々書きましたが、テストはあくまでテストです。

**テストを書いただけではコード自体の品質は改善しません。**

テストでコードが動作することを担保し、コードの大胆なリファクタリングを進めていくことが真に大切なことだと考えています。

私もこれらを意識して、プロダクトの品質改善に貢献していきたいです。

## 参考

https://www.praha-inc.com/lab/posts/unit-testing
https://www.praha-inc.com/lab/posts/testability
https://ja.wikipedia.org/wiki/%E5%8D%98%E4%BD%93%E3%83%86%E3%82%B9%E3%83%88
https://note.com/cyberz_cto/n/n26f535d6c575
https://thinkit.co.jp/article/14136
https://zenn.dev/taiga533/articles/e08ad4f4af5577079b5b