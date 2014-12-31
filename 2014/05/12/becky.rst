=============
 Beckyの設定
=============

これまでBecky!を利用していて困ったことはなかったのですが，メールサーバ
をCourier-IMAPサーバからDovecotに変更して気が付きました．

これまで，Courier-IMAPサーバで標準でIMAPのメールディレクトリPrefixに
「INBOX」が付いていたと思います．これで閲覧していると，受信箱の下に各
振り分けフォルダがぶら下がって見えるので，慣れもあってしっくりきていま
した．

見え方は

* 受信箱（20）

  - 00_振り分けフォルダ１
  - 01_振り分けフォルダ２

* 送信箱

  - 草稿
  - 送信済み
  - リマインダ

* ごみ箱

しかし，いつからBecky!のバージョンアップ以降でしょうか，もしくはBecky!
を入れ替えて設定がクリアされてしまったからでしょうか・・・
「ごみ箱」にアクセスすると

::

   0061 NO Client tried to access nonexistent namespace.  (Mailbox
   name should probably be prefixed with: INBOX.) 


といったようなエラーが発生するようになりました．

Dovecotの設定で「prefix = INBOX.」を削除してあげるとエラーは止められる
のですが，フォルダツリーの見え方が変わってしまいます．

どうするかというと，Becky!のサーバ設定＞詳細において．

::

   ごみ箱のフォルダ名：　INBOX.Trash
   草稿のフォルダ名：　　INBOX.Draft
   送信済みのフォルダ名：INBOX.Sent

これで解決です．

Thunderbirdの場合
=================

ちなみに，Thunderbirdの場合は，ごみ箱や送信済みトレイとして自動的に探
してくれます．しかし，Prefixに「INBOX」をつけていても，受信トレイの配
下にフォルダツリーが構成されません．

ためにし，受信トレイの下にフォルダを作成してみると．

::

   .INBOX.folderName

といったようなディレクトリが，Maildir配下に作成されてしまいます．
PrefixにINBOXが付いているので，そのままMaildir直下のディレクトリを受信
トレイ配下に並べてくれても良さそうなのですが・・・

まぁ，普段はWanderlustでしかメールを書くことがないので気にしない．

.. author:: default
.. categories:: Blog
.. tags:: dovecot
.. comments::
