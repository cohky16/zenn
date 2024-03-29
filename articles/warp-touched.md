---
title: "最近話題のWarpを触ってみた"
emoji: "🌀"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Warp", "terninal", "ターミナル"]
published: true
---

**21世紀のターミナル**

最近DevelopersIOで取り上げられたり、シードからシリーズAまでで約28億円もの資金調達をしたという記事が出たりと結構話題になっています。

https://dev.classmethod.jp/articles/warp-terminal/

https://techcrunch.com/2022/04/05/warp-raises-23m-to-build-a-better-terminal/

Warpはいわゆるターミナルアプリケーションで、一般的というか今まで広く使われてたのはiTermですかね。

確かにエディタとかは群雄割拠のような時代がありましたが(今もそう？)、ターミナルに関してはiTerm一強みたいな時代が今までずっと続いていたので、対抗馬的なのが出てくるのは不思議ではないですね。

これらの記事を見たときにWarpがターミナル史の歴史的転換点になりそうと思い、とてもワクワクしたので、触ってみることにしました。

## Warpについて

まずは公式サイトを見ます。とてもおしゃれ。

https://www.warp.dev/

今はMacOSしか対応していないみたいですね。今後WindowsとLinuxにも対応予定みたいです。

速度面でいうと、Rust製でとても早いらしいです。最近これをアピールポイントにしているツール多いですね。速度向上する上でやったことなどは以下に細かく書いてありました。

https://www.warp.dev/blog/how-warp-works

Warp特有の機能でいうと、コマンドをブロックで管理したり、GUIでコマンドのワークフローが組めたりします。

今後はターミナル内でドキュメントが実行できたり、ターミナルの共有ができるようになるみたいです。色々すごい！

あとはiTermでは色々なライブラリを組み合わせないとできなかったことがビルトインでできてます。例えばタブでの入力補完やgitブランチの表示、テーマなどです。便利すぎる。

## 導入

1. [ここ](https://app.warp.dev/download)からダウンロードして起動します。
2. GitHubのサインインを求められるのでサインインします。
3. 3つ質問が出るので答えます

これで導入は終わりです。

## 主要な機能

今回はWarp特有の機能を触っていきたいと思います。

### コマンドの改行

`Shift + Enter`　で改行

[![Image from Gyazo](https://i.gyazo.com/2dcf2e8d3373c7f4a3b638d633046cc2.gif)](https://gyazo.com/2dcf2e8d3373c7f4a3b638d633046cc2)

`^ + Shift + Up or Down`で複数行選択ができます。

[![Image from Gyazo](https://i.gyazo.com/733cfa59944e9ec03b19ff23437c90f5.gif)](https://gyazo.com/733cfa59944e9ec03b19ff23437c90f5)

まとめてコマンドを実行したい場合は便利です。

### コマンドブロック

Warpはコマンドの入力と出力をブロック単位で管理するため、コピペが簡単です。

[![Image from Gyazo](https://i.gyazo.com/b600db662bec65208a97473b05934e54.gif)](https://gyazo.com/b600db662bec65208a97473b05934e54)

また、`⌘ + Shift + S`でコマンドのリンクも作成することができるので、コマンドの共有も簡単にできます。

あとは、コマンドがブロックになっていることで、前の実行コマンドに遡るのも容易にできてすばらしいですね。

### ワークフロー

よく使うLinuxコマンドやcurlやgit、npm、dockerなど様々なコマンドがワークフローとして実行ができます。

`^ + Shift + R`でワークフローを表示して、使いたいコマンドを選択します。

[![Image from Gyazo](https://i.gyazo.com/bcae24516c607f7b0859007951ee3098.gif)](https://gyazo.com/bcae24516c607f7b0859007951ee3098)

自分でワークフローを登録することもできます。手順は[こちら](https://docs.warp.dev/features/workflows)を参照ください。

## まとめ

まだパブリックベータということで実運用には早いとは思いますが、実際に触ってみて、今までのターミナルの問題点が一気に解消されたような感じがあって私としてはとても期待が持てるなと思いました。これを機にターミナル業界がさらに盛り上がるといいですね。

~~(あとはiTermみたいにホットキーが設定できたら完璧)~~ -> [2022.3.30時点で実装されてたみたいですmm](https://docs.warp.dev/getting-started/changelog#2022.03.30-v0.2022.03.29.02.23)



