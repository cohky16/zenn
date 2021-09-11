---
title: "あなたのdotfilesを教えて！(随時更新)"
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
- Brewfile(HomeBrewの管理)
- lib/npm.sh(グローバルnpmパッケージの管理)
- initial.sh(完全な初期セットアップ時のシェルスクリプト)
- deploy.sh(シンボリックリンク作成のシェルスクリプト)

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

## system
alias v='vim'
alias e='exit'
alias sz='source ~/.zshrc'
alias ..2='cd ../../'
alias ..3='cd ../../../'

## package 
alias y='yarn'
alias yi='yarn install'
alias ya='yarn add'
alias n='npm'
alias ni='npm install'

## git
alias g='git'
alias gs='git status'
alias gc='git checkout'
alias gp='git push origin $(git rev-parse --abbrev-ref HEAD)'
alias gl='git pull origin $(git rev-parse --abbrev-ref HEAD)'
alias glm='git pull origin master'
alias t='tig'

## docker
alias dcu='docker-compose up -d'
alias dcd='docker-compose down --volumes'
```

### Brewfile(HomeBrewの管理)

HomeBrewで管理しているツールがまとまっているファイルです。

`brew bundle dump`でダンプ、`brew bundle install`でインストールができます。

### lib/npm.sh(グローバルnpmパッケージの管理)

グローバルでインストールしているnpmモジュールを管理するスクリプトです。

後述のinitial.shで呼び出すので関数にしてます。完全にハードコーディングなので改善の余地ありって感じです。

```shell
#!/bin/bash

run_npm() {
  npm i -g \
    @aws-amplify/cli \
    commitizen \
    cz-conventional-changelog \
    n \
    nodemon \
    npm \
    serve \
    serverless \
    ts-node \
    tsas \
    typeorm \
    typescript
}
```

### initial.sh(完全な初期セットアップ時のシェルスクリプト)

新規で環境構築する時に走らせるスクリプトです。

HomeBrewのツール、npmのモジュールをインストールします。

```shell
#!/bin/bash

set -eu

brew bundle install
source ~/dotfiles/lib/npm.sh
run_npm
./deploy.sh
```

### deploy.sh(シンボリックリンク作成のシェルスクリプト)

新規でdotfileを追加しても変更が不要になるようにfor文で回して、サブモジュールとiTerm2の設定ファイルを適用するコマンドも入れています。

plistファイルはシンボリックリンクでやろうとするとうまく行かなかったので単純にコピーするようにしました。ここ地味に詰まった…

```shell
#!/bin/bash

set -eu

for f in .??*
do
  [[ $f == ".git" ]] && continue
  [[ $f == ".DS_Store" ]] && continue
  [[ $f == ".vim" ]] && continue
  [[ $f == ".config" ]] && continue
  echo "$f"
  ln -sf ~/dotfiles/$f ~/
done

git submodule update -i
cp -f iTerm2/com.googlecode.iterm2.plist ~/Library/Preferences
```

現状の私はこのような感じです。
starshipとか.vimのカラースキーム、.zshrcの設定などは人によってかなり変わるとこだと思うので、そこらへんを中心にコメントしていただけると助かります〜





