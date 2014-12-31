=================
 Lubuntuのすゝめ
=================

Lubuntuに特化した話ではないですが，Ubuntuを使う場合に私はこんな設定を
している・・・という内容を紹介します．

インストール
============

簡単すぎるので省略します．インストールディスクさえマウントできれば，あ
とは淡々とインストールするだけなので．

そのうち，物理マシンのHDDではなくUSBにインストールする方法でも考えたい
な・・・と．

まずはじめにアップデート
========================

インストールしたら必ずやることと．

Debian系のパッケージ管理といえば「apt-get」ですが，これが少し拡張され
た「aptitude」を利用しています．そこまで大きな違いは無いようですが，パッ
ケージの依存関係が管理されており，パッケージを削除する際には，関連パッ
ケージもまとめて削除してくれるようです．

プロキシの設定
--------------

会社で利用していると，プロキシ経由でなければインターネット側に抜けられ
ないのでプロキシの設定をします．RedHat系で「yum」を利用するのであれば，
環境変数（http_proxy）でも書いておけばいいのですが，Ubuntuでは不足なの
で設定を追加します．

::

   $ sudo vi /etc/apt/apt/apt.conf.d/00proxy
   Acquire::http::proxy "http://proxy.server.co.jp:8080;
   Acquire::https::proxy "http://proxy.server.co.jp:8080;
   Acquire::ftp::proxy "http://proxy.server.co.jp:8080;

更新の実行
----------

あとは「aptitude」を準備して，更新を実行するだけです．

::

   $ sudo apt-get update
   $ sudo apt-get install aptitude
   $ sudo apt-get autoclean
   $ audo aptitude update
   $ audo aptitude safe-upgrade
   $ auto aptitude autoclean

時間がかかりますが，ほっとけば終わります．

必要なパッケージの追加
======================

::

   $ sudo aptitude install openssh-server
   $ sudo aptitude install vim

必要になったタイミングでおいおい準備していきます．

ホストOS（Windows）ディレクトリの参照
=====================================

Lubuntu側のメーラ（Emacs）でメールを受けて，添付ファイルをホストOS側に
渡して，Office製品で編集する．なんてパターンが多々あります．Windows側
からLubuntu側のファイルをSamba経由で参照しても良いのですが，私の場合は
Lubuntu側からWindowsのファイルシステムをCIFS経由で参照しています．

その方法は・・・

::

   $ sudo aptitude install cifs-utils
   $ sudo aptitude install autofs
   $ sudo vi /etc/auto.master
   ・・・追記・・・
   /mnt/host   /etc/auto.host
   
   $ sudo vi /etc/auto.host
   Users   -fstype=cifs,rw,uid={USER},gid={GID},credentials=/home/{USER}/.host.cifs     ://host/Users
   
   $ vi ~/.host.cifs
   username={USER}
   password={PASS}
   $ chmod 400 ~/.host.cifs

こんな感じで，AutofsでホストOS側の共有フォルダを参照するようにします．
もちろんホストOS側では，C:\Uesrs以下を共有名「Users」で共有しておく必
要があります．CIFSの認証用ファイルをホームディレクトリ配下に配置してお
きます．パーミッションに気をつけましょう．（どうせ自分しかVMさわらない
ですけどね）

さらに，Windowsのホームディレクトリ「c:\Users\username\」配下のよく利
用するディレクトリへのシンボリックリンクを作成しておくと便利です．

::

   $ ln -s /mnt/host/host/Users/{USER}/Desktop ~/
   $ ln -s /mnt/host/host/Users/{USER}/Downloads ~/
   $ ln -s /mnt/host/host/Users/{USER}/Documents ~/

これで，メーラから添付ファイルを保存する先を「~/Desktop」などに指定す
れば，さくさくWindows側からファイルを操作することが可能です．

ここでは，autofsによる自動マウントで実現していますが，「/etc/fstab」に
記述してしまっても良いかもしれません．ただ，パスワードをそのまま書くの
は気持ち悪いので，「crendentials」だけ別ファイルに切り出したほうが，少
しだけ安心できます．


.. author:: default
.. categories:: Linux
.. tags:: lubuntu
.. comments::
