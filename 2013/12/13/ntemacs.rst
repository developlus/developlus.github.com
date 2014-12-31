=================
 NTEmacsのすゝめ
=================

Windows版のEmacs（Ntemacs/gnupack emacs）を利用する際の便利な設定を紹
介します．
既にCygwinを利用していることが前提ですが，Cygwinがなくても応用できるか
と・・・

NTEmacsのインストール
=====================

NTEmacsはIME用のパッチが，gnupackから提供されており，かつパッチのみを
適用したNTEmacsが `こちら <http://cha.la.coocan.jp/doc/NTEmacs.html>`_
から提供されています．

もしくは， `gnupack <http://sourceforge.jp/projects/gnupack/>`_ から
Emacs単体でも提供されているので，どちらを利用しても構わないと思います．

取得したEmacsのパッケージを回答して，Cygwinから[/usr/local/emacs/24.3]
みたいなディレクトリに格納します．バージョンアップごとにLispを差し替え
るのは面倒なので，以下のようにシンボリックリンクを貼ってくと便利です．

::

   drwxr-xr-x+ 1 24.3/
   lrwxrwxrwx  1 bin -> /usr/local/emacs/24.3/bin/
   lrwxrwxrwx  1 etc -> /usr/local/emacs/24.3/etc/
   lrwxrwxrwx  1 info -> /usr/local/emacs/24.3/info/
   lrwxrwxrwx  1 leim -> /usr/local/emacs/24.3/leim/
   lrwxrwxrwx  1 lisp -> /usr/local/emacs/24.3/lisp/
   lrwxrwxrwx  1 site-lisp -> /usr/local/emacs/24.3/site-lisp/

[site-lisp]までリンクにしていますが，site-lispはバージョンごとに共通で
問題無いと思います．

**2014/01/28：追記**

上記のようにシンボリックで対応すると，各種Lispをインストールする際に正
しくインストールできません．そのため，Windowsのリンク機能（ジャンクショ
ン）で対応する必要があります．詳細については，次の節を参照してください．



Windowsのディレクトリに細工
===========================

EmacsからるとCygwin上のファイルパスとWindows上のファイルパスが異なるの
で，Windows側に細工してあげます．

Windowsのコマンドプロンプト（スタートメニュー＞すべてのプログラム＞ア
クセサリ＞コマンドプロンプト）を管理者権限（右クリックして「管理者とし
て実行」）で起動します．

Windowsでいうディレクトリへのシンボリックリンクのようなもの，ジャンク
ションを設定します．

::

   mklink /J usr c:\cygwin\usr
   mklink /J home c:\cygwin\home
   mklink /J bin c:\cygwin\bin

こうしておくことで，Emacsからファイルを開く際に，Cygwinから見えるファ
イルパスと，Windowsから見えるファイルパスがほぼ同じに見えるので，厄介
事が少なくなります．

IMEの有効化
===========

NTEmacs上でIMEの入力を便利にするために，EmacsのIMEパッチを有効にする必
要があります．

::

   ;; Win32IME用のパッチを有効化
   (w32-ime-initialize)
   (setq default-input-method "W32-IME")
   ;;モードライン行の表示
   (setq-default w32-ime-mode-line-state-indicator "[--]")
   (setq w32-ime-mode-line-state-indicator-list '("[--]" "[あ]" "[--]"))
   ;;カーソルカラー
   (add-hook 'input-method-activate-hook   ;; IME ON/OFF時のカーソルカラー
      (lambda () (set-cursor-color "firebrick")))
   (add-hook 'input-method-inactivate-hook ;; IME OFF時の初期カーソルカラー
      (lambda () (set-cursor-color "white")))

これで完了．

.. author:: default
.. categories:: Emacs
.. tags:: emacs,cygwin
.. comments::
