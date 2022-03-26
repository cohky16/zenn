---
title: "Goの1.18を触る"
emoji: "🐣"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Go"]
published: true
---

1.18での目玉機能であるgenericsとfuzzを軽く触っていきます
  - https://go.dev/doc/tutorial/generics
  - https://go.dev/doc/tutorial/fuzz

## 準備
- Goを1.18にアップデートするだけ
  - brewなら`brew install go`
  - 公式
    - https://go.dev/doc/install

## generics
### 概要
- 一言でいうと総称型
  - 実際に利用されるまで型が確定しない
  - Go特有のものではないので目新しさとかは特にない
### やってみる
- intとfloatで型が違うだけで処理は同じ関数をそれぞれ作成する
  ```go: main.go
  // SumInts adds together the values of m.
  func SumInts(m map[string]int64) int64 {
      var s int64
      for _, v := range m {
          s += v
      }
      return s
  }

  // SumFloats adds together the values of m.
  func SumFloats(m map[string]float64) float64 {
      var s float64
      for _, v := range m {
          s += v
      }
      return s
  }

  func main() {
      // Initialize a map for the integer values
      ints := map[string]int64{
          "first":  34,
          "second": 12,
      }

      // Initialize a map for the float values
      floats := map[string]float64{
          "first":  35.98,
          "second": 26.99,
      }

      fmt.Printf("Non-Generic Sums: %v and %v\n",
          SumInts(ints),
          SumFloats(floats))
  }
  ```
- genericsを使って共通化できる
  ```go:main.go
  // SumIntsOrFloats sums the values of map m. It supports both int64 and float64
  // as types for map values.
  func SumIntsOrFloats[K comparable, V int64 | float64](m map[K]V) V {
      var s V
      for _, v := range m {
          s += v
      }
      return s
  }
  ```

## fuzz
### 概要
- いわゆるfuzzテスト
  - 異常系のテストを自動化して確認する
### やってみる
- 文字列を反対にする関数を作る
    ```go: main.go
    func Reverse(s string) (string, error) {
	    b := []byte(s)
	    for i, j := 0, len(b)-1; i < len(b)/2; i, j = i+1, j-1 {
	    	b[i], b[j] = b[j], b[i]
	    }
        return string(b)
    }

    func main() {
	    input := "The quick brown fox jumped over the lazy dog"
	    rev := Reverse(input)
	    doubleRev := Reverse(rev)
	    fmt.Printf("original: %q\n", input)
	    fmt.Printf("reversed: %q\n", rev)
	    fmt.Printf("reversed again: %q\n", doubleRev)
    }
    ```
  - メモ
    - main関数にある`The quick brown fox jumped over the lazy dog`ってなんだと思ったらすべてのアルファベットを使う有名な言葉遊びらしい
    - https://ja.wikipedia.org/wiki/%E3%83%91%E3%83%B3%E3%82%B0%E3%83%A9%E3%83%A0
- `go test -fuzz=Fuzz`でfuzzing
  - 文字列をバイトで逆順にするとマルチバイトが無効になってしまうのでruneで処理する
    - https://go.dev/blog/strings
    ```go:main.go
    func Reverse(s string) (string, error) {
	    r := []rune(s)
	    for i, j := 0, len(r)-1; i < len(r)/2; i, j = i+1, j-1 {
	    	r[i], r[j] = r[j], r[i]
	    }
	    return string(r)
    }
    ```
  - メモ
    - テストがfailした場合、次にfuzzフラグを外せば同様のテストケースが実行できる
      - テストケースがtestdata/fuzz/${FuzzTestName}に保存されている
    - `t.Logf()`でテストがfailしたときかvオプションをつけたときのみ標準出力することができる
- 再度`go test -fuzz=Fuzz`でfuzzing
  - UTF-8以外の文字が入るとエラーになるのでバリデーションを組み込む
    ```go: main.go
    func Reverse(s string) (string, error) {
	    if !utf8.ValidString(s) {
	    	return s, errors.New("input is not valid UTF-8")
	    }
	    r := []rune(s)
	    for i, j := 0, len(r)-1; i < len(r)/2; i, j = i+1, j-1 {
	    	r[i], r[j] = r[j], r[i]
	    }
	    return string(r), nil
    }
    ```
  - メモ
    - `go test -run ${FuzzTestName}/${filename}`で特定のファイルにあるテストケースを実行できる
- 再度`go test -fuzz=Fuzz`でfuzzing
  - failはしていないが処理が止まらない
  - fuzzingは明示的に停止しない限り、failするまで実行される
      - `-fuzztime 30s`のように時間を指定する
        ```shell
        ❯ go test -fuzz=Fuzz -fuzztime 5s
        fuzz: elapsed: 0s, gathering baseline coverage: 0/43 completed
        fuzz: elapsed: 0s, gathering baseline coverage: 43/43 completed, now fuzzing with 12 workers
        fuzz: elapsed: 3s, execs: 757072 (252347/sec), new interesting: 2 (total: 45)
        fuzz: elapsed: 5s, execs: 1242894 (228431/sec), new interesting: 2 (total: 45)
        PASS
        ok  	example/fuzz	5.251s
        ```

## 感想

今回Go最大のアップデートということで主要な2つの機能を触りましたが、実際のプロダクトで即戦力として扱えるレベルだと感じました。

Go公式のチュートリアルはわかりやすくていいですね。また新機能がきたら触りたいと思います。