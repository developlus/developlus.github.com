===============
 Emacsのすゝめ
===============

ほぼメーラ専属となってしまいましたが，Emacs＋Wanderlustでメールの読み
書きをしています．今回はLubuntuで，Emacs24とWanderlust2.15系の組み合わ
せで，メールを読み書きするまでの手順を紹介します．

Emacsのインストール
===================

Emacsのインストール自体はお手軽です．Emacs23系でも十分に満足していたの
ですが，Emacs24系からLispパッケージの管理が楽になるとのことで，Emacs24
を使うことにします．

::

   # aptitude install emacs24 emacs-mozc

「emacs-mozc」はかな漢字変換を行うLispです．中身は「mozc_server」と
「mozc_emacs_helper」というサーバクライアントプロセス間で通信して，
Emacsがかな漢字変換結果を受け取るシステムのようです．
Mozcは野良コンパイルは面倒なので，パッケージ管理（aptitude）でさくっと
入れてしまいます．

その他，使っているLispはこんなもの．

まず初めにinit-loader.elする
----------------------------

前は，「.emacs」に記述していましたが，WindowsでNtemacsを使ったり，
Linux上でEmacs使ったり設定の切り替えが面倒になってきました．
最初は頑張って

::
   
   (cond
     ((and (equal system-type 'windows-nt) (= emacs-major-version23))
       (load "~/.emacs.d/init-ntemacs23.el))
     ((= emacs-major-version 23)
       (load "~/.emacs.d/init-23.el))
     ((= emacs-major-version 24)
       (load "~/.emacs.d/init-24.el))
   )

みたいなことを書いていたのですが，プラットフォーム毎に各々のファイルを更新しなければならないためストレスいっぱいでした．

そこで目をつけたのが「init-loader.el」です．

::

  (when (locate-library "init-loader")
    (require 'init-loader)
    (setq init-loader-show-log-after-init nil)
    (init-loader-load "~/.emacs.d/inits"))

「~/.emacs.d/inits」ディレクトリ配下に配置されているLispを次々とロード
してくれます．プレフィックスに「linux-」が付いているとLinuxプラットフォー
ムでだけ読み込む．「windows-」が付いているとWindowsプラットフォームで
だけ読み込む．あとは，プレフィックスに数字２桁をつけると番号が若い順に
読み込むようです．

詳細は， 
`こちら(.emacs.d配下) <https://github.com/developlus/home/tree/master/.emacs.d/>`_ 
に公開しているので実際に見てもらったほうが早いかと．

Wanderlustのインストール
========================

Ubuntuのパッケージ管理（aptitude）でも簡単にWanderlusutをインストール
できるのですが，せっかくなので最新版をインストールする手順をご紹介．

Wanderlustの製作者であり配布元である 
`本家 <http://www.gohome.org/wl/index.ja.html>`_ 
は更新が途絶えております．いまは，Github上で管理されているのでこちらを
取得します．

::

   # cd /usr/local/src/
   # git clone https://github.com/wanderlust/wanderlust
   # git clone https://github.com/wanderlust/apel
   # git clone https://github.com/wanderlust/flim
   # git clone https://github.com/wanderlust/semi
   # cd apel
   # make
   # make install
 
という具合に，「apel/flim/semi/wanderlust」の順番でインストールしてい
くのですが，何も考えずにインストールしようとすると，Lispの格納先が
「/usr/local/share/emacs/site-lisp」および，
「/usr/local/share/emacs/24.3/site-lisp」になります．
変更したければ，「make」時の引数に「LISPDIR=」などでインストールパスを
変更することが可能です．どこの環境でも同じものを利用したいのであれば
「~/.emacs.d/site-lisp」に配置しておくのもありかと思います．詳細にてつ
いては「Makefile」の先頭を確認してください．
私の場合は，環境を変えたら最新のものを使いたいので，毎度インストールし
なおしていますね．

なお，Lisp配置先のディレクトリで再帰的に「load-path」に組み込めるよう
に「subdirs.el」というファイルを「site-lisp」ディレクトリ配下に作成し
ておきます．

::

   # cat /usr/local/share/emacs/site-lisp/subdirs.el 
   ;; -*- no-byte-compile: t -*-
   (if (fboundp 'normal-top-level-add-subdirs-to-load-path)
     (normal-top-level-add-subdirs-to-load-path))

バージョン依存の「/usr/local/share/emacs/24.3/site-lisp/」のほうも同様
です．これを置いておかないと，「flim，semi」を続けてバイトコンパイルす
ることができないと思います．
もちろん，「init.el」で「load-path」に含めておいてください．

あとは，ホームディレクトリに「.wl/.folder/.address/.signature」などの
ファイルを配置すれば準備完了です．

「.wl」は先人の知恵がたくさんWeb上に転がっているので参考にさせてもらい
ました．私が使っているのは 
`こちら(${HOME}配下) <https://github.com/developlus/home/>`_ 
に配置しています．

細かな設定の内容については別の機会に・・・

.. author:: default
.. categories:: Emacs
.. tags:: emacs,Wanderlust
.. comments::
