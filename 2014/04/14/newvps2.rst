==================================
 サーバリニューアルのすゝめ（２）
==================================

Emacs24を使う
=============

Ubuntu 12系では残念ながら標準のapt-getリポジトリにはEmacs24が準備され
ていない．ググるとヒットするのですが，手順を書いておくと・・・

::

   # aptitude install python-software-properties
   # add-apt-repository ppa:cassou/emacs
   # aptitude update
   # aptitude install emacs24 emacs24-el

さて，ここからが問題です．どうやってMozcを利用するのか？emacs-mozcを
apt-getからインストールしてしまうと，emacs23もインストールしてしま
う・・・かといって，野良ビルドするのもイヤだし・・・あぁ野良ビルド版を
パッケージっぽく管理してくれるツール [#f1]_ があったな・・・とか

さんざん悩んだ挙句．



::

   # aptitude install emacs-mozc

してみました．結果，Emacs23もインストールしてしまったものの，emacsのデ
フォルトパスもemacs24に向いており，emacs24からmozc.elを呼ぶことが出来
たのでなんの問題もありませんでした．

.. rubric:: 脚注
.. [#f1] paco（http://porg.sourceforge.net/）

.. author:: default
.. categories:: Blog
.. tags:: emacs, vps
.. comments::
