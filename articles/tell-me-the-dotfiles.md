---
title: "あなたのdotfilesを教えて！"
emoji: "🙏"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["dotfiles"]
published: true
---

「いちいち設定ファイル作るのめんどくせ〜〜」

ということでdotfilesなるものを作りました。

https://github.com/cohky16/dotfiles

作ってみたものの、物足りなさや、周りはどのようなdotfilesなんだろうと思ってこの記事を書くことにしました。

コメントにどしどし書いていただけるともれなく私が喜びます。

教えてください！！だけだとあれなので私のdotfileの説明もちょろっと書いておこうと思います。

構成としては、

- starship(プロンプトをおしゃれにするやつ)
- .vim(カラースキーム)
- iTerm2(iTerm2の設定)
- .vimrc(vimの設定)
- .zshrc(zshの設定)
- init.sh(シンボリックリンク作成のシェルスクリプト)

一つずつ詳細書きます。

### starship(プロンプトをおしゃれにするやつ)

とにかく見やすくおしゃれにしてくれます。
何もしなくてもブランチ表示してくれたり便利で簡単です。
公式がとてもわかりやすいので導入はそっち見たほうが良いと思います。
ただ私には色が眩しすぎたのでiTerm2で色をいじってます。

![starship](https://i.gyazo.com/1a73659cd5af3020bc5b8494fb9f813a.png)

https://starship.rs/ja-JP/

### .vim(カラースキーム)

hybridがいい感じだったので使ってます。
使用頻度高くないのでそこまでこだわりはありません。

![hybrid](https://i.gyazo.com/d837e238df36e8c92169598cb8dd0c56.png)

### iTerm2(iTerm2の設定)

私の場合、optionを2回押すと上からターミナルが降ってくる的な感じにしてます。

![iTerm2](https://i.gyazo.com/7e776574f4dcc433ef25dc2156294b68.gif)

#### 降ってくる設定

```keys -> hotkey -> Create a Dedicated Hotkey Window...```

でDouble-Tap Keyをoptionにするだけです。controlボタンとかも設定できます。

#### 半透明の設定

```profile -> Hotkey window -> window```

でtransparencyとかblurをいじると変更できます。お好みで。

#### デフォルトのウィンドウで開かないようにする設定

```General -> window restoration policy```

only restore hotkey windowにすることでhotkeyのみで開く設定にできます。

### .vimrc(vimの設定)

カラースキームを有効にしただけです。
いいのがあれば増やしていきたい、と思ってはいる。思ってるだけ。

```vim
let g:hybrid_use_iTerm_colors = 1
colorscheme hybrid
syntax on
```

### .zshrc(zshの設定)

割とガッツリ設定している人が多い印象のファイルです。
私はstarshipの設定と頻繁に使うコマンドのaliasだけです。ここ充実させたいです！！！

```vim
eval "$(starship init zsh)"
export GO111MODULE=on

# alias
alias yi='yarn install'
alias yr='yarn run'
alias gp='git pull origin'
```

### init.sh(シンボリックリンク作成のシェルスクリプト)

特に言うことなし。

```shell
#!/bin/bash

set -eu

ln -sf ~/dotfiles/.vimrc ~/.vimrc
ln -sf ~/dotfiles/.zshrc ~/.zshrc
ln -sf ~/dotfiles/.config ~/.config
ln -sf ~/dotfiles/.vim ~/.vim
bash ./iTerm2/init.sh
```

現状の私はこのような感じです。
starshipとか.vimのカラースキーム、.zshrcの設定などは人によってかなり変わるとこだと思うので、そこらへんを中心にコメントしていただけると助かります〜





