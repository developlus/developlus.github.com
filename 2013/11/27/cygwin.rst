================
 Cygwinのすゝめ
================

普段の生活でターミナルが無いと生活しにくいものです．まだまだ，TeraTerm，
を使っているひとも少なくないと思いますが・・・

かれこれ１０年以上愛用している「Cygwin ＋ ckターミナルエミュレータ」が
おすすめです．もはやこれを手放すことができません．Windows上で生活して
いながら，かゆいところに手が届きます．

あとは，Windows版Emacsがサクサク動いてくれれば，Linux環境を手放すこと
ができそうです．

Cygwinのインストール
====================

Cygwinの現在（2013年11月）の最新バージョンは1.7系です．しかも，64bitバ
イナリも提供されています．私の環境は，Windows7 64bitにCygwin
1.7.25（32bit）です．

インストールの流れは省略しますが，私が使っているパッケージを参考までに
紹介しておきます．

- Devel

  * gcc-core　・・・ちょっとコンパイルするときに
  * gcc-g++　・・・ちょっとコンパイルするときに
  * git　・・・gitからソース落とすときに
  * make　・・・ちょっとコンパイルするときに
  * subversion　・・・ソース管理するために

- Editors

  * vim　・・・何はなくとも必要

- Net

  * inetutils　・・・nslookupとか含まれてたはず
  * nc　・・・簡易ファイル転送とか使えるから
  * openldap　・・・Emacs＋Wanderlustのアドレス帳検索で使う
  * openssh　・・・何はなくとも必要

- Utils

  * ncurses　・・・ターミナルエミュレータ上でclearとかするので

- Web

  * w3m　・・・Emacs＋w3m-modeで使う

- X11

  * xorg-server　・・・LinuxのEmacsを転送してくる

Cygwinの環境変数
================

なくても問題無いかもしれませんが，環境変数「HOME」を設定します．環境変
数の設定方法は省略しますが

::

   HOME  =  C:\Cygwin\home\UserName

と設定されていれば問題ないかと．

ckのインストール
================

`Cygwin ck terminal emulator <http://www.geocities.jp/meir000/ck/>`_
はCygwin上で動作するターミナルエミュレータです．
何年か前に突如配布サイトが消失してしまって，この世の終わりかと思いまし
たが，現在は定期的に更新されています．ありがたい限りです．

配布ファイルを展開し，「ck.app.dll/ck.con.exe/ck.exe」の３点のファイル
をCygwinインストールディレクトリ配下の「/bin」にコピーします．
さらに「ck.exe」のショートカットを作成しましょう．ショートカットのリン
ク先を

::

   C:\cygwin\bin\ck.exe -dir C:\cygwin\home\UserName

のようにしておくと，起動直後にホームディレクトリをカレントディレクトリ
として設定することが可能です．

また，ホームディレクトリに「.ck.config.js」を配置すると動作設定を細か
に設定することが可能です．
詳細については，配布ファイルに含まれているドキュメントを確認してくださ
い．

ckの便利な設定
==============

私がckをカスタマイズしている部分を紹介します．

::

   Config.window.position_x = -1;
   Config.window.position_y = 0;
   Config.window.cols = 120;
   Config.window.rows = 65;
   
   Config.window.transparent     = 0xCC;
   
   Config.window.font_name = "MS Gothic";
   // Config.window.font_name = "MeryoKe_Console";
   // ※MeiryoKe_consoleはWindows8以降で含まれる6.12でないと
   //   利用することができないらしい・・・
   
   Config.window.color_foreground    = 0x00CC00;
   
   Config.accelkey.paste            = Keys.CtrlL  | Keys.ShiftL | Keys.P;
   // Ctrl+Shift+p で貼り付け，Ctrl+yでもいいかも
   
   function user_cmd_ime_off(window){
     if(window.ImeState)
     window.ImeState = false;
     return false;
   }
   AccelKeys.add(Keys.Escape, user_cmd_ime_off);
 
ck上でvimを開くことが多いのですが，IMEがonになっていると，vimのコマン
ドモードで反応してくれなくなるのですが，「Esc」を押してコマンドモード
に移行した瞬間にIMEをoffにすることが可能ですので，vim＋日本語環境の煩
わしさから少しだけ開放されます．

Xサーバの自動起動
=================

LinuxからEmacsウィンドウを転送してくるために，Cygwin付属のXサーバを常
時起動させておきます．貧弱なマシンだと少々重いかもしれませんが，私は気
にしません．

::

   %appdata%\Microsoft\Windows\Start Menu\Programs\Startup

に，X起動用のショートカット作成します．起動用のショートカットは，「ス
タートメニュー＞すべてのプログラム＞Cygwin-X＞XWin Server」を参考にし
てください．リンク先は

::

   C:\cygwin\bin\run.exe /usr/bin/bash.exe -l -c /usr/bin/startxwin.exe

このような感じです．

また，デフォルトでXサーバを起動すると，無駄にXtermが起動されたりするの
で，デフォルトでは何も起動しないように設定するため，Cygwinのホームディ
レクトリに「.startxwinrc」および「.XWinrc」という空ファイルを配置して
ください．すると，Xサーバのみが単体で起動されます．

Emacsの呼び出し
===============

Windows上の仮想マシンとして起動しているLinuxのEmacsを呼び出します．
Linux側にX転送設定を書いても良いのですが，面倒ですのでSSHサーバのX転送
機能を利用します．といっても，特に設定は不要のはず・・・

Windows上に下記のようなショートカットを作成してください．

::

   C:\cygwin\bin\run.exe /usr/bin/bash.exe -l -c  ~/bin/remacs

さらに，ホームディレクトリ配下の「bin」ディレクトリに，emacsというシェ
ルスクリプトを作成します．

::

   $ cat ~/bin/remacs
   #!/bin/sh
   ssh -Y -C vm.hostname.jp emacs > /dev/null 2>&1 &

あとは，ショートカットを起動すると，Emacsが呼び出されてくるはず．ちな
みに，Linuxにsshする際に「-Y」オプションをつけてログインしていれば，
「firefox」や「LibreOffice」のウィンドウも転送することが可能です．普段，
メールをWanderlustで読んでいる時に，添付ファイルのExcelなんかは，Emacs
上で開いてLibreOfficeで閲覧していたりします．

Office関連のファイルの編集はMS Officeでやった方が良いですが，閲覧だけ
ならLibreOfficeで十分ですね．

.. author:: default
.. categories:: Cygwin
.. tags:: cygwin,emacs
.. comments::
