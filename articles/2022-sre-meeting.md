---
title: "「6社合同 SRE勉強会」で学んだこと"
emoji: "💡"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["SRE"]
published: true
---

以下イベントを見てたときのメモ的なあれです。SREの初学者目線で学べたことを書きました。全ては見れてません。

https://line.connpass.com/event/236497/

## 全体通して学んだこと

- 「SREとは」といったところから定義しているところが多かった
  - こうしないとインフラエンジニアの名称が変わっただけのただの便利屋になってしまう
  - 何ができて何をするのか
    - https://blog.studysapuri.jp/entry/sre-vision-mission-values
- SREはSREでも種類があったりした
  - CenterOfPractice(CoreSRE、pureSRE、横断SREなど)
    - 全社としてのSRE
    - ベストプラクティスの策定
    - ツールの選定や開発
  - EmbeddedSRE
    - プロダクト開発チーム内としてのSRE
      - Embeddedだと外から埋め込むという意味合いになってしまうので、内部からの場合はenablingといった表現をしている会社もあった
    - CoPが作る文化を実践して広める
    - 実際に導入してのCoPへのフィードバック
  - 以下参考
    - https://medium.com/slalom-build/the-many-shapes-of-site-reliability-engineering-468359866517
- 実際に手を動かすというよりSRE文化の醸成に努めている会社が多いイメージだった
  - ベストプラクティスを作って流行らせるみたいな
  - SREを理解してもらうために勉強会を開いたり
  - プロダクトごとにSRE習熟度シートみたいなのを作ってどこまでできているか評価するとか
    - SLO、監視、インシデント、ポストモーテム、トイルの撲滅など

## 各トラックで学んだこと

-  約9倍のスパイクに備える取り組み(LINEスタンプのあけおめLINE)
   -  発表
      -  LINE
   -  アジア圏を中心にしているので日本、台湾、タイで1時間毎に3回ピークがある
   -  アクセス予測が難しい
      -  方法として成長率を軸に考える
         -  年平均成長率
         -  通常時のアクセス数の前年比
      -  予測精度が高いと思われる
         -  CPU最大70%程度のスペック
      -  不安がある
         -  CPU最大50%程度のスペック
   -  やってよかったこと
      -  障害発生した際のワークショップ
         -  いわゆるカオスエンジニアリング
         -  開発環境で行なっているらしい
-  CyberZのSREにおけるプロダクト開発チームとの関わり方
   -  発表
      -  CyberZ
   -  SRE本に沿ってSRE文化の醸成に努めている感じ
      -  https://www.amazon.co.jp/SRE-%E3%82%B5%E3%82%A4%E3%83%88%E3%83%AA%E3%83%A9%E3%82%A4%E3%82%A2%E3%83%93%E3%83%AA%E3%83%86%E3%82%A3%E3%82%A8%E3%83%B3%E3%82%B8%E3%83%8B%E3%82%A2%E3%83%AA%E3%83%B3%E3%82%B0-%E2%80%95Google%E3%81%AE%E4%BF%A1%E9%A0%BC%E6%80%A7%E3%82%92%E6%94%AF%E3%81%88%E3%82%8B%E3%82%A8%E3%83%B3%E3%82%B8%E3%83%8B%E3%82%A2%E3%83%AA%E3%83%B3%E3%82%B0%E3%83%81%E3%83%BC%E3%83%A0-%E6%BE%A4%E7%94%B0-%E6%AD%A6%E7%94%B7/dp/4873117917/ref=sr_1_1?__mk_ja_JP=%E3%82%AB%E3%82%BF%E3%82%AB%E3%83%8A&crid=35YUYUUIMWA8J&keywords=google+sre&qid=1647147233&sprefix=google+sr%2Caps%2C170&sr=8-1
   -  バリューストリームマッピング
      -  各開発プロセスに要する時間を可視化してボトルネックを見つける
      -  どの企業でも導入すべきだと思った
      -  miroを使ってリモートにも対応したらしい
      -  https://note.com/cyberz_cto/n/n8fd89e3a5828
-  横断的なSRE推進と成熟度評価
   -  発表
      -  サイバーエージェント
   -  SREsとしての取り組み
      -  SREとしての定義から始める
      -  現在地を知る
         -  モニタリングはできているかなど
         -  SREヒエラルキーを下段から
         -  https://sre.google/sre-book/part-III-practices/
      -  バックエンドとの信頼を勝ち取ることが大事
         -  信頼性を謳うエンジニアが信頼ないのは本末転倒
   -  強制というより流行を作りたい
   -  SREは外交官
      -  https://www.infoq.com/presentations/sre-build-trust/
-  SRE を実現するための組織マネジメント
   -  発表
      -  リクルート
   -  0回目のポストモーテム
      -  プレモーテム
      -  障害に対して事前にどう対応するかを実践しておく
   -  開発チームで一部SREを担当してもらう
      -  partially
      -  enablingという考え
      -  開発チーム視点でSREの実践、文化の醸成ができる
   -  文化は中から(embedded)、技術的なものは外部(pure)
   -  以下書籍を参考に話していた
      -  https://www.amazon.co.jp/gp/product/4820729632/ref=ox_sc_act_title_1?smid=AN1VRQENFRJN5&psc=1
