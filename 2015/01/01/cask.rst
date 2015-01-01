======
 Cask
======

これまでLubuntuでapptitude（apt-get）でLispをインストール出来ていたの
で，必要最低限のLispパッケージは自分でインストールしていました．特に
Wanderlustとか・・・

el-getでパッケージ管理できることは知っていたので，el-getでやるか・・・
とググってみると，Caskというパッケージ管理が目につきます．

Emacs24からパッケージ管理が組み込まれたと言えども，インストール指定な
ければ毎度エラーが出るし．

Caskでやれば，パッケージの管理もアップデートも一発ということで，こちら
でCaskを試してみる．

Caskのインストール
==================

まずは `Official Web <https://github.com/cask/cask>`_ を参照．こちらに
インストール方法も記載してあります．

::

   > curl -fsSkL https://raw.github.com/cask/cask/master/go | python

本当に楽です．
なお，もしMacを使っているならば，これでもOKで紹介されています．

::

   > brew install cask
   
Caskのセットアップ
==================

`こちら <http://cask.readthedocs.org/en/latest/>`_ のドキュメントに使
い方が紹介されています．

PATHを設定する
--------------

::

   export PATH="$HOME/.cask/bin:$PATH"

いっそ「.bashrc」などに記述しておく．

あとは

::

   > cask upgrade-cask

を実行するように記載されています．cask自体をアップデートする模様．

セットアップ
------------

以下の手順でセットアップする模様

::

   > cask init

そして，Emcasの「.emacs.d/init.el」に以下を設定．

::

   (require 'cask "~/.cask/cask.el")
   (cask-initialize)

さらに，「.emacs.d」以下に「Cask」というファイルに必要なパッケージを記
述していく．とりあえずデフォルトで記述されているファイルのみでOKであれ
ば．

::

   > cask install

これだけで完了．
ただ，意外とほしいパッケージは登録されていなかったりする．

とりあえず使えるパッケージで普段から使っていた下記のパッケージをインス
トールする．

::

   (depends-on "wanderlust")
   (depends-on "init-loader")


.. Author:: default
.. categories:: Emacs
.. tags:: cask
.. comments::
