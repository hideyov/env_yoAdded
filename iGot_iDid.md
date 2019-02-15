# 環境構築やミドルウェア導入のメモ

## 190117 Node.js インストール

udemy のECMAScript基礎講座のため、Visual Code studio をインストール。プラグインのquokkaを使うためにNode.js 環境が必要であった。

$ node -v --> v11.6.0  
$ npm -v --> 6.8.0

$ which node --> /usr/local/bin/node  
$ which npm --> /usr/local/bin/npm

## 190214 Sass導入のためgulpをインストール

rubyのgemでのSassインストールがエラー解決できず、node-sass(LibSass)を導入

gulp-cli をグローバルにインストール

npm install --global gulp-cli

$ gulp -v --> [10:41:19] CLI version 2.0.1

$ which gulp --> /usr/local/bin/gulp

## Node.jsのバージョン管理ツールndenvをインストール

※ 順序が逆になった。すでにhomebrewでインストールしていたNode.jsを、ここでアンインストールしておくべきだろうと思われる

https://qiita.com/noraworld/items/462689e108c10102d51f

ndenv を使用して複数のバージョンの Node.js を管理する方法と基本的な使い方

### リポジトリをクローン

git clone https://github.com/riywo/ndenv.git ~/.ndenv

### パスを通す

$ echo 'export PATH="$HOME/.ndenv/bin:$PATH"' >> ~/.profile

$ echo 'eval "$(ndenv init -)"' >> ~/.profile

※ 説明では .bash_profileへの追記になっている

パスを通したらシェルを再起動して環境変数を反映させる。

$ exec $SHELL -l

$ which ndenv --> /Users/hideyo_pr/.ndenv/bin/ndenv

### node-buildを導入する

このままでもndenvコマンドは使用できるが、肝心な、Node.jsをインストールする ndenv install コマンドが使用できないので、ndenv installを使用するためのプラグイン node-buildをインストール。まずはリポジトリをクローン。

$ git clone https://github.com/riywo/node-build.git $(ndenv root)/plugins/node-build

$ git clone https://github.com/riywo/node-build.git /Users/hideyo_pr/.ndenv/plugins/node-build

※ $(ndenv root) が展開されて /Users/hideyo_pr/.ndenv （おそらく）

これでndenv 導入は完了。

### インストールできるバージョン一覧を確認

インストールできる Node.js/io.js のバージョン一覧を確認するには

$ ndenv install -l

インストールしたバージョンが最新で、一覧に出なければ、リポジトリを最新のものにアップデート

$ cd ~/.ndenv

$ git pull

すでにhomebrewでインンストールしていた Node.js(v.11.6.0)をアンインストール

$ brew uninstall node

これで、node -v としても --> env: node: No such file or directory

### ndenv で、あらためてNode.jsをインストール

$ ndenv -v -->  ndenv 0.4.0-5-g6ee4840

$ ndenv install v11.6.0

インストールされたnodeのバージョン確認：$ ndenv versions

※ この時点では $ node -v -->  -bash: /usr/local/bin/node: No such file or directory

### 複数バージョンのNode.jsをインストールして、バージョン設定（グローバル）

v11.6.0 と v9.10.0 と v8.2.0 をインストールし、デフォルトで使用するバージョンをv11.6.0に設定。

$ ndenv global v11.6.0

※ ここで $ node -v としても怒られる。terminalを再起動すると

$ node -v --> v11.6.0

$ ndenv versions --> 

* v11.6.0 (set by /Users/hideyo_pr/.ndenv/version)

  v8.2.0
  
  v9.10.0
  
$ npm -v --> 6.5.0-next.0

### Node.jsのバージョン設定（ローカル）

特定のフォルダで、ノードのバージョンを設定する

$ mkdir hoge

$ cd hoge

$ ndenv local v8.2.0

$ node -v --> v8.2.0


