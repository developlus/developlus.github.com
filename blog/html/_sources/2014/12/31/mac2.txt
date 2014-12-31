==========
 Homebrew
==========

Mac初心者がいろいろとMacを便利に使う方法を調べた経緯でたどり着いたパッ
ケージ管理システム＝Homebrew．

「Mac パッケージ管理」でググると一つ目に出てくるので説明する必要は不要
なはずですが，自分のための備忘録として作業記録を残しておく．一つ目の目
標は，MacでもTinkererを使えるようになること．


Homebrewのインストール
======================

何はなくとも，まずはHomebrewをインストールする．・・・と言いたいところ
だけれども，Macの開発環境であるXcodeが必要．

Mac初心者の私にとっては，Xcodeって何？という感じですが，iOSのアプリを
作ったりするためには必須という統合開発環境ですね．今はSwiftが流行りだ
とか・・・

App StoreからXcodeを見つけてインストールします．私はハマりましたが，
Xcodeをインストールしただけでは意味がなくて，一度起動してUser
Agreementに同意しましょう．

そして，'Homebrew <http://brew.sh/>'_ をインストールします．インストー
ル方法は，左記のOfficial Webの下段に記載していあるインストールスクリプ
トを実行するだけです．最近は，インストールスクリプトをWebからダウンロー
ドして直接インストールするパターンのやつ多いですね．Sublime Text
Editorでも経験したので，そろそろ違和感がなくなってきました．

実行するスクリプトを以下の通り．

::

   > ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"


これだけでインストールは完了なはずなのですが，うろ覚えだけれども，多分
何かしらコマンドを実行しろと言われるはず・・・特に，Xcode関連のコマン
ドを要求されます．特に注意すべきは下記

::

   > xcode-select --install

これを実行することで，コマンドライン用のDeveloper toolsがインストール
されるとのこと．詳しくは「man xcode-select」オプションを確認するとよろ
しい

あとは，パッケージリストのアップデート等々を実施して，必要なパッケージ
をインストールしていく．

::

   > brew update
   > brew install {PackageName}


   
Pythonのインストール
====================

MacにデフォルトでインストールされているPythonでも特に問題はないはずで
すが，ライブラリ関連でトラブりそうなのでHomebrewでインストールしたもの
に統一しておく．

::

   > brew install python

標準で「eazy_install」がついてくるので，pipを入れておく．というか個人
的にpipのほうしか使っていない．ついでにpip-toolsもインストールしておく．

::

   > type easy_install
   easy_install is /usr/local/bin/easy_install
   > sudo easy_install pip
   > pip install pip-tools
   > pip-review


「pip-review --auto」で実行すると，pipで管理しているパッケージを自動的
にアップデートする．ただ，β版であろうと容赦なくアップデートするので注
意が必要．

Tinkererのインストール
======================

ブログを更新するために，Tinkererをインストールする．

::

   > pip install tinkerer

あ．忘れていた．肝心なgitもインストールしておく．

::

   > brew install git

非常に便利です．Macってこんなに便利なんだ・・・

あとは，Linuxを使っているのと同じように，ブログ記事を更新していくだけ
です．

::

   > cd repos/
   > git clone https://github.com/developlus/developlus.github.com

いつもどおり！！
   
.. author:: default
.. categories:: Mac
.. tags:: mac, tinkerer, homebrew
.. comments::
