================
 Tinkererの設定
================
Tinkerrの初期設定については，様々なサイトで紹介されているので省略する．
ただし，頭が真っ白な状態では現状のサイト構成辿りつけなかったのでポイン
トだけ記録しておきます．

日本語設定
==========

ビルドについて
--------------

::

 $ tinkerer -b

を事項すると，Unicode関連のエラーが発生する場合があった．そもそも
Pythonに慣れていないので，この事象が意味不明だったが単純に文字列の定義
に誤りがあった．

::

 $vim conf.py
 ---snip---
 description = '脳内情報'  # コンパイルエラー
 description = u'脳内情報' # 文字列がUnicodeであることをマークする

記事中の日本語について
----------------------

これらの記事はEmacs（rst-mode）で記述しており，「auto-fill-mode」を有
効にしています．なので，ReStドキュメント本体は適切な箇所で自動改行され
ています．

この状態で何も考えずにコンテンツをビルドすると，改行が挿入された箇所が
不自然に半角スペースに変換された状態となり，サイトの見た目が美しくあり
ません．どうせビルド時に改行を[&nbsp;]に置換しているんだろうな・・・と
はあたりがつくものの，どこの設定を調節すれば良いのかサッパリでしたが，
おおよそ自分が悩んでいる事象は世の中の先人も悩んでいるはずです．

そんなわけで，ちょっとインターネットの力を借りると・・・これ

::

 $ mkdir _exts
 $ cd _exts
 $ wget http://dl.dropbox.com/u/218108/files/japanesesupport.py
 $ cd ../
 $ vi conf.py
 ---snip---
 extensions = ['tinkerer.ext.blog', 'tinkerer.ext.disqus',
 'japanesesupport']

これだけで完了です．

詳細についてはこちら・・・
http://www.python.jp/pipermail/sphinx-users/2012-July/000303.html

テーマについて
==============

デフォルトのテーマでは味気ない．そこでテーマを変更します．デフォルトで
提供されているテーマは以下の通り．

::

 $ ls -l /usr/local/lib
 $ ls -l /usr/local/lib/python2.7/dist-packages/tinkerer/themes/
 合計 28
 drwxr-sr-x 4 root staff 4096 11月  5 11:59 boilerplate
 drwxr-sr-x 3 root staff 4096 11月  5 14:56 dark
 drwxr-sr-x 3 root staff 4096 11月  5 11:59 flat
 drwxr-sr-x 3 root staff 4096 11月  5 11:59 minimal5
 drwxr-sr-x 3 root staff 4096 11月  5 11:59 modern5
 drwxr-sr-x 3 root staff 4096 11月  5 11:59 responsive

私は，「dark」を利用しています．しかし，少々見にくいというか，見出しが
単調なので少しだけCSSを変更して，オリジナルテーマとしました．（ライセ
ンス的にOKなのか確かめてないのですが，きっと大丈夫・・・）

オリジナルテーマは「_themes」というディレクトリに配置して置くと良いです．

.. code-block:: css

 $ mkdir _themes
 $ cp -rfp /usr/local/lib/python2.7/dist-packages/tinkerer/themes/dark ./developlus
 ---Modified developlus/static/dark.css
 $ diff original-dark/static/dark.css  developlus/static/dark.css 
 355a356
 >     text-decoration: underline;
 359a361
 >     background: #ccc;
 382a385,392
 > /* Custom Developlus */
 > .widget h1 {
 >     font-size: 16px;
 >     border-left: #ccc solid 6px;
 >     border-bottom: #ccc solid 2px;
 >     padding-left: 0.4em;
 > }
 > 
 388c398
 <     padding: 30px 0;
 ---
 >     padding: 20px 0;
 393a404,434
 >     border-left: #666 solid 8px;
 >     border-bottom: #666 solid 2px;
 >     text-decoration: none;
 >     padding-left: 0.4em;
 > }
 > 
 > .main article h2 {
 >     font-size: 1.2em;
 >     line-height: 1.2;
 >     border-left: #666 solid 6px;
 >     border-bottom: #666 dashed 2px;
 >     text-decoration: none;
 >     padding-left: 0.4em;
 > }
 > 
 > .main article h3 {
 >     font-size: 1em;
 >     line-height: 1.2;
 >     border-left: #666 solid 2px;
 >     border-bottom: #666 dotted 1px;
 >     text-decoration: none;
 >     padding-left: 0.4em;
 > }
 > 
 > .main article p {
 >     padding-left: 1em;
 > }
 > 
 > .main article pre {
 >     margin-left: 1em;
 >

こんな感じです．

.. author:: default
.. categories:: Linux
.. tags:: tinkerer,sphinx
.. comments::
