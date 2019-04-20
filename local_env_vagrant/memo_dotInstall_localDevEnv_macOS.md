# vagrant_ローカル開発環境の構築 [macOS編](2019/4/19)

dotInstall に従って vagrant 環境構築を成功させ、マニュアル化を試みる。
一度インストールしたが環境構築に失敗している。=> 181225いったん挫折_vagrantインストール.txt
macBookProにインストールしたvagrant はアンインストールする。=> vagrantアンインストール.txt

## ローカル開発環境（ローカルサーバー）とは

開発中は手元の PC にこれと同じ環境を作っておいて、テストや開発ができれば便利。今回 VirtualBox というツールを使っていく。これがあれば、手元の PC の中にいくつもサーバーを立ち上げることができる。

VirtualBox は便利だが、サーバーの設定や OS のインストール作業などをするのが面倒。そのあたりを簡単なコマンドで扱えるようにするのが Vagrant というツール。

今回どちらもインストールしていくが、直接扱っていくのは Vagrant だけ。 VirtualBox は直接扱っていかないので実態は見えにくいが、Vagrant のコマンドを通じて裏で VirtualBox が動いている、というイメージ。

なお、ローカル開発環境を作るのではなくて、もっと手っ取り早くサーバー環境を用意して PHP や Ruby を勉強したい場合は、ブラウザ上でそうした環境を作ってくれる Cloud9 というサービスもおすすめ。

### MacOS に VirtualBox をインストール

VirtualBox-6.0.6-130049-OSX.dmg が最新版。（コースでは、VirtualBox-5.1.6）

ちなみに古いバージョンがあったらそのまま上書きインストールしてくれるだけなので、どんどん進めていけば OK。

管理者用のパスワードを聞かれるので mac を購入した時に設定したパスワードを入力

インストールが終了すると、インストーラもインストール用ウィンドウも不要。デスクトップに VirtualBox のアイコンが出たままになっているが、それも不要。

### Vagrant をインストール

2019/4/19時点で最新バージョンは Vagrant 2.2.4 

すでにインストールしていた vagrant 1.9.0 をアンインストールするため、下記ディレクトリ、リンクを削除。

/usr/local/bin/vagrant (symbolic link) -> /opt/vagrant/bin/vagrant  /Users/hideyo_pr/.vagrant.d/

ダウンロードしたVagrant-2.2.4をインストール。$ vagrant -v => Vagrant 2.2.4

### 構築作業の流れを確認

今回、macOS 上に Vagrant で立ち上げていくのですが、サーバー 1 つにつき 1 つのフォルダが必要。今回はよくサーバーの OS で使われる CentOS で作っていきたいので MyCentOS というフォルダを作る。

今後複数のサーバーを立ち上げることも想定。例えばもう 1 つサーバーを立ち上げたかった場合、MyProject などのフォルダを作りたくなるかと。MyVagrant というフォルダを作って、その中でサーバーを管理するフォルダを管理する。次にこのフォルダの中でサーバーを立ち上げていくのですが、サーバーの設定ファイルを Vagrant のコマンドで作っていく。

具体的には Vagrantfile というファイルを作り、サーバーの設定をする。今回 IP アドレスの設定だけをしておく。ip アドレスは他のマシンとかぶらないように設定すればいいので、とりあえず 192.168.33.10 にしておく。もし 2 台目のサーバーを立ち上げる場合には、こちらでも Vagrantfile を作って、例えば 192.168.33.11 といった具合にすれば OK。

ここまでできたら、あとは Vagrant のコマンドを使ってあげると Vagrantfile の設定を読み込んで、裏で VirtualBox を立ち上げてサーバーを作ってくれる、という仕組みになっている。

### 仮装マシンを立ち上げる

[caution !!!] 以下、補足情報 => 「動画の最初に実施している vagrant-vbguest プラグインの導入はエラーを引き起こす可能性があるため、実施しないようにしてください。」

$ cd, $ mkdir MyVagrant ：複数の仮想マシンを入れる MyVagrant というフォルダを作成。

$ cd MyVagrant, $ mkdir MyCentOS, $ cd MyCentOS ：今回作る仮想マシンを入れるフォルダ MyCentOS を作成して移動。

vagrant init bento/centos-6.8 ：仮想マシンの設定をする Vagrantfile を作成。>>>

"A `Vagrantfile` has been placed in this directory. You are now ready to `vagrant up` your first virtual environment! Please read the comments in the Vagrantfile as well as documentation on `vagrantup.com` for more information on using Vagrant."

Vagrantfile が作成されると以上のメッセージが出る。そして、ホームディレクトリに、 .vagrant.d/ という隠しフォルダが作られる。

【参考】なお、bento/centos-x.x 部分は使用する Vagrant Box の名前。これは、Vagrant を提供する HashiCorp 社が同じく提供する、Atlas というサービスに登録されている Vagrant Box の名前を指定する。 https://atlas.hashicorp.com/boxes/search 

ユーザ名/Vagrant Box 名 という規則になっており、bento は Chef 社がメンテナンスするオーガナイゼーション (ユーザ名)。Chef 社は「Chef」という構成管理ツールを展開している、信頼できる配布元といえる。

参考 http://neos21.hatenablog.com/entry/2017/04/17/074117

Vagrantfileを編集して仮想マシンのIPアドレスを192.168.33.10にする

sed -i '' -e 's/# config.vm.network "private_network", ip: "192.168.33.10"/config.vm.network "private_network", ip: "192.168.33.10"/' Vagrantfile

上記スクリプトにより、Vagrantfile の35行目の部分が下記のようにコメントアウトされる。

config.vm.network "private_network", ip: "192.168.33.10"

### 仮想マシンを起動

$ vagrant up ：このコマンドにより、裏でvirturalBox が動いて仮装マシーンを起動する。（だいたい5分ぐらいの感じ）

==> default: Successfully added box 'bento/centos-6.8' (v2.3.4) for 'virtualbox'!

などと表示されて間もなく成功！（サーバーが立ち上がった）

$ vagrant status >> running (virtualbox) 

### ユーザーからの質問 - CentOS 7 はサポート対象外（この講座では）

CentOS 7 にするには最初に vagrant init で指定した「bento/centos-6.8」の指定を変更すればよいのですが、ローカル開発環境の構築に使用しているスクリプトが CentOS 6 環境を前提としているため、 CentOS 7 にされてしまうと環境の自動構築ができなくなります。

また、ドットインストールのレッスン動画はすべて CentOS 6 の環境を前提とした内容となっております。そのため、CentOS 7 にされた場合に発生するエラー等はサポート対象外となってしまいますので、CentOS 6 のままご利用ください。

### ユーザーからの質問 - sudo をつけないと vagrant up がうまく実行されない

「vagrant up とするとエラーが出力され、CentOSが立ち上がりません。しかしsudo vagrant upとすれば起動できています。なぜなのでしょうか。」

過去にそのディレクトリで sudo をつけて vagrant コマンドを実行してしまったため、ディレクトリの所有者の設定がおかしくなっていると推測されます。新しくディレクトリを作成して、そのディレクトリで改めてローカル開発環境を作り直してみてください。新しいローカル開発環境では sudo なしで vagrant up ができるかと思います。

### vagrant up でエラー出てもビビらない

一度、下のエラーが出て止まったが、再度 vagrant up すると何事もなかったかのように成功した。

An error occurred while downloading the remote file. The error
message, if any, is reproduced below. Please fix this error and try again.

OpenSSL SSL_read: SSL_ERROR_SYSCALL, errno 60

### 仮装マシンの設定をする

$ clear でターミナルの画面をクリア。

Vagrantfile のあるフォルダで vagrant ssh => 立ち上げたサーバーにログイン

サーバーにログインすると、コマンド入力部分が [vagrant@localhost ~]$ に変わる。vagrant から始まれば、仮装マシーンのサーバーにログインしていることになる。自分が今Macを操作しているのか、それとも仮装マシーンを操作しているのかは、ここで判別できる。

サーバーが立ち上がって、OSがインストールされているが、まだPHPやRubyなどのアプリケーションがインストールされていない。こちらでよく使いそうなアプリケーションを自動でインストールするためのスクリプト ./run.sh を用意しておきました。

[vagrant@localhost ~]$ sudo yum -y update ：OSを最新状態にアップデート（時間かかる。10分ぐらいの感じ）

[vagrant@localhost ~]$ sudo yum -y install git ：スクリプトを入手するための gitをインストール

[vagrant@localhost ~]$ git clone https://github.com/dotinstallres/centos6.git ：gitを使ってアプリケーション設定用スクリプトをDL

生成された centos6 フォルダに移動して、スクリプトを実行 >>> [vagrant... ~]$ ./run.sh 

ネットワーク状況によっては、途中で止まったように見えることもある。エラーのようなものが出ても、taskが終了していればOK。（macBookAirに、アパートのWi-Fiでは30分以上かかった。）

[vagrant... ~]$ exec $SHELL -l ：最後に、もろもろの設定を反映させる。(-1ではない。エル)

### ./run.sh では具体的に何がインストールされているのか（ユーザーからの質問）

./run.sh を実行すると色々なプログラムがインストールされるようですが、結局どのソフトウェアがインストールされたのでしょうか？

./run.sh で実施している内容につきましては、今後変更される可能性もあるため、下記URLを参考にしてみてください。こちらのファイルの「- name:」の行に説明があります。例えば「- name: install apache」というのは Apache をインストールしていることを示しています。

https://github.com/dotinstallres/centos6/blob/master/main.yml

### Cyberduck をインストール

仮想マシンの設定が終わったので、仮想マシン上のファイルを簡単に扱えるようにするファイル転送ツールを導入。今回は Cyberduck というフリーのツールを使う。公式サイト https://cyberduck.io/

Cyberduck は Vagrant や VirtualBox と違ってインストーラーが付属しないので、Finder で表示してインストール作業を進める。zipファイルを展開してプログラム本体を取り出し、Application フォルダに移動。

### Cyberduck の設定

環境設定[cmd + ,] > [ブラウザ] >>> ['.'で始まるファイルを表示、ダブルクリックしたファイルを外部エディターで開く]にチェック

外部エディターなども設定し、再起動

### 仮想マシンにアクセス

cyberduckを起動 > [新規接続] 

転送プロトコル：SFTP（SSHによる暗号化FTP）/ サーバ名：[192.168.33.10 (今回設定した)] 

ユーザ名とパスワード：ともに vagrant （Vagrantで立ち上げたサーバーには、小文字の vagrant ユーザーが作られ、パスワードも同じ）

「キーチェーンに追加する」にチェック > 接続 > 

「そのホストは現在システムに認識されていません」と出るが、ここで「許可」とすると仮想マシンに接続することができる。「常に」にチェックを忘れずに入れておきましょう。

接続に成功すると、カレントディレクトリが /home/vagrant と表示される。

同時に、$ ~/MyVagrant/MyCentOS に移動して $ vagrant ssh でログイン後に [vagrant@localhost ~]$ pwd => /home/vagrant

つまり、home フォルダの下の vagrant フォルダが、vagrant ユーザのホームフォルダになる。[!!! caution]

後でアクセスしやすいように今の接続を Cyberduck のブックマークに入れておく。[新規ブックマーク] 

### PHPの学習をしてみる

ホームディレクトリ /home/vagrant に php_lessons フォルダを作成。index.php に <?php echo "Hello!" 

この命令をブラウザで実行するためには、ターミナルから「ウェブサーバー」を立ち上げる必要がある。PHPのビルトイン・サーバーで確認する。

[vagrant@localhost php_lessons]$ php -S 192.168.33.10:8000 => PHPが用意しているウェブサーバーが起動

Listening on http://192.168.33.10:8000 と出るので、ブラウザにURLを貼って確認。停止は ctrl + c

### 仮想マシンからログアウト(exit)してから停止(vagrant suspend)

ブラウザ、ファイル転送ツール(Cyberduck)、立ち上げたエディターは、そのまま終了させて大丈夫。

ターミナルでPHPのウェブサーバーが立ち上がっていれば、Ctrl + C で止める。

仮想マシンからのログアウトは [vagrant@localhost...]$ exit => 仮装マシンからMacに、操作が戻る。

$ vagrant suspend => 仮想マシンを停止。

何らかの原因でターミナルをそのまま閉じてしまったり、PC を終了させてしまったとしてもそれほど問題はないが、[ exit で仮装マシンからのログアウト > vagrant suspend で仮想マシン停止] が基本！

$ vagrant suspend , $ vagrant status => saved (virtualbox) 

### 学習再開の手順

$ cd MyVagrant/MyCentOS ：仮想マシンのあるフォルダに移動。

$ vagrant up ：仮想マシンが（停止していれば）起動。

$ vagrant ssh ：仮装マシンにログイン。=> [vagrant@localhost ~]$ に変わる

例えばrubyについて学習するため新規フォルダ ruby_lessons を作成。新規ファイル hello.rb を作成。[ puts "this is ruby" ]

ターミナルで $ ruby hello.rb を実行して確認。

### ファイル転送ツールTransmitを使ってみよう

https://panic.com/blog/ja/hello-transmit-5/

### hosts ファイルを編集する

仮装マシーンの、数字だらけのアドレスに名前をつける。

hosts（ホスツ）とは、TCP/IPを利用するコンピュータにおけるホスト名のデータベースで、IPアドレスとホスト名の対応を記述したテキストファイルである。(wikipedia)

/etc/hosts に、新たな設定 192.168.33.10 dev.dotinstall.com を追記。（既にある設定は、いじらない！）

$ php -S 192.168.33.10:8000 でPHPのウェブサーバーを立ち上げ、設定したURLにアクセス。

これはあくまで、手元の開発環境にわかりやすい名前をつけるための設定。他のデバイスからはこのURLは通用しない。

[!!! caution] なお、mvim で編集しようとすると、w! などでの強制保存も効かず、 $ sudo vi hosts で編集・保存ができた。

### ポート番号 :8000 つけ忘れに注意（ユーザーからの質問）

hosts ファイルを編集後、 Apache のテストページが表示されてしまいます。なぜでしょうか？

URL の最後に「:8000」をつけ忘れているようです。 dev.dotinstall.com でなく dev.dotinstall.com:8000 にアクセスしてみてください。
