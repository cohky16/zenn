---
title: "インターネットの基礎知識まとめ"
emoji: "💻"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["internet"]
published: true
---

最近以下のロードマップを参考に、今ある知識の見直しとこれから必要な知識の確認をしています。

https://roadmap.sh/backend

今回はわかっているつもりになっていた、インターネットについてまとめてみました。

## インターネットとは

一言でいうと、コンピューター同士を接続したネットワークのこと。

通信プロトコルと概念モデルを使用して接続を可能にしている。

- 通信プロトコル
   - 概要
      - コンピューター同士を接続する上での約束事
   - 種類
      - HTTP
         - WebサーバーとクライアントのWebブラウザでHTMLなどのデータを送受信するためのプロトコル
      - HTTPS
         - SSL/TLSを用いてHTTPの暗号化通信を行うためのプロトコル
         - 厳密には通信プロトコルではなく、あくまでhttpの拡張
      - FTP
         - コンピューターからサーバーへのデータ転送のためのプロトコル
         - HTTPと違い、Webページに関連するものだけでなく様々なものを転送する際に使用される
      - IP
         - グローバルなネットワークに接続されているすべてのコンピューターに対してIPアドレスと呼ばれる固有番号を付与し、IPアドレスを使用して通信先の指定や呼び出しを行うために使用されるプロトコル
      - Ethernet
         - LANによる接続の際に使用されるプロトコル
      - DNS
         - ドメイン名をIPアドレスに変換する際、DNSサーバーとのやりとりを行うために使用されるプロトコル
      - などなど...
- 概念モデル
   - 概要
      - 通信機能の分割モデルのこと
      - 上位レイヤーは下位レイヤーに依存する
         - 例えば、httpはtcpがないと機能しない
   - 種類
      - TCP/IPモデル
         - 4層で分割されているモデル
      - OSI参照モデル
         - ISOが定義した7層で分割されているモデル

   ![TCP/IPモデルとOSI参照モデルの画像](https://cdn-xtech.nikkei.com/atcl/nxt/mag/nnw/18/031400131/031400001/01.jpg?__scale=w:500,h:406&_sh=03e0a30f80)*https://xtech.nikkei.com/atcl/nxt/mag/nnw/18/031400131/031400001/* 

https://xtech.nikkei.com/atcl/nxt/mag/nnw/18/031400131/031400001/

## ブラウザとは

ユーザーがGUIを介してWebページやその他のオンラインコンテンツにアクセスして表示できるようにするソフトウェアのこと。

ブラウザは以下の要素で構成されている。
- ユーザーインターフェース
   - そのまま。アドレスバーやブックマークなどのこと。
- ブラウザエンジン
   - UIとレンダリングエンジンの間でデータ形式の変換などを行う要素。
- レンダリングエンジン
   - コンテンツの表示を行う要素。HTMLやCSSの解析を行い、画面に表示する。
- ネットワーキング
   - httpなどのネットワークリクエストを処理する要素。
- UIバックエンド
   - ウィジェットの描画などを処理する要素。
- JavaScriptインタプリタ
   - JavaScriptのコードをパースしたり実行したりする要素。
- データストレージ
   - Cookieなどのデータを保存しておく要素。

![ブラウザの構成要素の画像](https://web-dev.imgix.net/image/T4FyVKpzu4WKF1kBNvXepbi08t52/PgPX6ZMyKSwF6kB8zIhB.png?auto=format&w=1000)*https://web.dev/howbrowserswork/#the-browser's-high-level-structure* 

https://web.dev/howbrowserswork/

## ドメイン名とは

一言でいうと、人間が読みやすい形式のネットワークアドレスのこと。

ネットワークのアドレスにはIPアドレスが使用されるが、人間が覚えづらく、読みづらい。

IPアドレスからドメイン名への変換はDNSサーバーが行い、通信プロトコルにはDNSが使用される。

https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_domain_name

## ホスティングとは

一言でいうと、サーバーを借りること。

htmlやcssなどのWebウェブサイトのコンテンツはオンラインで表示できるように、サーバーに格納しておく必要がある。 

https://www.namecheap.com/hosting/what-is-web-hosting-definition/

https://boxil.jp/mag/a2442/




