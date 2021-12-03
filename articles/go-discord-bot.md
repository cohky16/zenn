---
title: "GoでDiscordBotを作る"
emoji: "🐭"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Go", "Discord"]
published: true
---

社内のコミュニケーションアプリとしてDiscordを使っているのですが、通話を依頼する際に毎回「今通話いいですか？」みたいに入力するのと文言考えるのが面倒だと感じたので、コマンド叩くだけで音声チャンネルへの招待リンクを作成してくれるボットを作成しました。

https://github.com/cohky16/invite

今回はGoの勉強がてらGoを使って開発しようと思い、色々調べたのですが情報があまり多くなかったので、ここに諸々書き残しておこうと思います。

## DiscordDeveloperPortalでアプリ作成

まずはDiscordDeveloperPortalでアプリを作成します。

https://discordapp.com/developers

### Bot作成

アプリを作成したら、次にBotを作成します。

作成したら以下をやっておきます。
- token発行
- 権限の設定をする
- SERVER MEMBERS INTENTを有効にする

権限の設定とSERVER MEMBERS INTENTについて詳しく説明します。

権限の設定に関しては、Bot画面の下に`Bot Permissions`という色々選択できる画面があるので、そこで付与するアクセス権限を押して行きます。

最終的に`PERMISSIONS INTEGER`という数字が発行されるのでコピーしておきます。サーバーに追加する際に使用します。

SERVER MEMBERS INTENTは、GUILD_MEMBERSという値をBotが参照できるようにするための設定です。Guildがサーバーという意味合いなので、そのサーバーにいる全ユーザーを取得して何か処理をしたいときにはここを有効にしないと取得できないので注意です。私はここで詰まりました。

### サーバーにBotを追加

Botの作成、設定が終わったら後はサーバーに追加するだけです。

以下のURLに作成したアプリのclient_id、権限の設定を入れてブラウザでアクセスすれば認証画面に遷移します。

```
https://discordapp.com/oauth2/authorize?client_id=YOUR_CLIENT_ID&scope=bot&permissions=0
```

画面の指示に従ってサーバーにBotを追加します。これで下準備は完了です。

## 開発

ここからは開発についてざっと説明します。

### 使用するライブラリ

ライブラリはdiscordgoというライブラリを使います。(gopherが変形しすぎてて怖いです)

https://github.com/bwmarrin/discordgo

使い方はとても簡単です。

discordgoをインストール
```
go get github.com/bwmarrin/discordgo
```

discordgo.Newでクライアントを生成
```go
import "github.com/bwmarrin/discordgo"

discord, err := discordgo.New("Bot " + "authentication token")
```

あとはこのクライアントを使ってBotを開発するだけです。

### オーバービュー

網羅的に説明するため、まずはコードの全体像を説明します。

説明のためエラーハンドリングは削除しています。

```go
func main() {
  	discord, err := discordgo.New("Bot " + "authentication token")
  
	discord.AddHandler(onMessageCreate)

	err = discord.Open()

	stopBot := make(chan os.Signal, 1)

	signal.Notify(stopBot, syscall.SIGINT, syscall.SIGTERM, os.Interrupt, os.Kill)

	<-stopBot

	err = discord.Close()

	return
}
```

### 各コードの説明

```go
discord.AddHandler(onMessageCreate)
```

ここでハンドラの設定をしています。discordgoでのハンドラの形式は以下のようになっています。

```go
func onMessageCreate(s *discordgo.Session, m *discordgo.MessageCreate)

// 形としては以下
func (s *discordgo.Session, m *discordgo.$(eventName))
```

sessionには文字通りセッション情報が色々入っていて、messageCreateにそのハンドラを叩いたユーザーやメッセージなどの情報が入ってきます。

```go
err = discord.Open()
```

ここでwebsocketを使って通信を開始します。

```go
stopBot := make(chan os.Signal, 1)

signal.Notify(stopBot, syscall.SIGINT, syscall.SIGTERM, os.Interrupt, os.Kill)

<-stopBot
```

何かしらのエラーシグナルを検知したタイミングで処理を終了するようにしています。

```go
err = discord.Close()
```

ここで通信をクローズするようにしてます。クローズがfailする可能性があるためdeferを使っていません。

## まとめ

ドキュメント見ると色々なことができそうなので、是非色々作ってみてください。

https://pkg.go.dev/github.com/bwmarrin/discordgo

今回GoでDiscordBotを作ってみて、Goの書きやすさなどのメリットや、普通の言語にはあるはずのアサーションとかがデフォルトになくてテストが書きづらいなどGoで結構言われているメリットデメリットを体験することができたので、とても勉強になりました。

今度はGoでcliツールとか作ってみたい


