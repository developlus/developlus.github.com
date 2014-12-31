================
 Emacs on MacOS
================

Mac OS上でEmacsを使う．これまで，Emacsに関する情報収集するときに，よく
「Cocoa Emacs」という単語を目にしていましたが，ついに自分が使う時がき
たか・・・

Emacsをインストールする
=======================

といっても，Homebrewでサクッとインストールすることができる．

::

   > brew install emacs

これだけでインストールすることが可能です．
なお，これだけだと「/usr/local」配下にインストールされるだけなので，ま
ともに利用することはできない．

::

   > brew linkapps

これを実行すると，「/Applications」配下にシンボリックリンクが作成され
るので，ランチャからEmacsを起動することができる．

ただ，自分の環境だけなのか？Spotlight検索からEmacsが検索されなかった．
「/usr/local/Cellar/emacs/24.4/Emacs.app」を「/Application」配下にコピー
することで解決されるが，手動でコピーするのが気持ち悪かったので，自分の
ホームディレクトリ配下にある「/home/{UserName}/Application」配下に，
「Emacs.app」をコピーして利用することにした．これで，ランチャにも表示
されるし，Spotlight検索でも起動できる．

キーバインド
============

Macのキーボードは特殊・・・というか，Macでは普通なことですが．ちなみに
私は，JPキーボードで利用しています．信者な方には，「ハハッ」と言われる
でしょうが，もう「＠（アットマーク），「＿（アンダースコア）」のキー配
置で混乱してしまうので，JPキーボードでいいのです．Ctrlも標準で「A」の
隣に配置されているし．

ちなみに，WindowsではHHKのJPを利用しているので，なんら違和感はありませ
ん．

Emacsを使っていて困るのが，「Command」と「Option（Alt）」の配置です．
HHKでも左手親指を微妙に左にずらした位置・・・つまりCommandキーの位置が
Altなわけです．EmacsでMetaキーの役割を果たすAltキーの位置は重要なわけ
で，これを解決するキーバインドが必須です．

これまで「init-loader」を利用していたので，これを利用して「Cocoa Emacs」
の場合のキーバインドを設定します．

::

   > emacs .emacs.d/conf/cocoa-emacs-keybind.el

   ;; Command ⇔ Option
   (setq ns-command-modifier (quote meta))
   (setq ns-alternate-modifier (quote super))


これで万事OKです．

MacでEmacsを使い始めて，これまで利用していた「.emacs.d」ディレクトリ配
下が，そのままでは利用できないことが判明したので，色々と変更していきま
す．その記事はまた別の記事へ．


.. author:: default
.. categories:: Emacs
.. tags:: emacs, mac
.. comments::
