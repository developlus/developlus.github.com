==================================
 サーバリニューアルのすゝめ（４）
==================================

パケットリピータ（Stone）
=========================

Stoneを利用するとプロキシ経由でしたインターネットにアクセスできない環
境から外部のサーバにSSH出来たりします．

褒められたことはではありませんが，会社などセキュリティ条件が厳しい環境
からでも，外部にトンネル掘れるわけですね．

早速インストール・・・といっても，Ubuntuだと標準でリポジトリに・・・

::

   # aptitude install stone

あとは起動スクリプト的なものを準備しておくだけです．ただし，会社のプロ
キシがHTTPSな接続の許可ポートが443しかないので，あたかもHTTPS通信をし
ているように見せかけるためには443でListenする必要があります．

うまく設定すれば，本物のHTTPSとStoneを切り分けこともできるようですが，
面倒なのでそこまでは求めません．


起動スクリプト（/etc/init.d/stone）
===================================

::

   #! /bin/sh
   PATH=/sbin:/usr/sbin:/bin:/usr/bin
   DESC="Stone Server"
   NAME=stone
   DAEMON=/usr/bin/$NAME
   DAEMON_ARGS="-z certkey=/etc/ssl/stone/stone.pem -l -D localhost:10022 443"
   SCRIPTNAME=/etc/init.d/$NAME
   KILL=/usr/bin/killall
   
   [ -x "$DAEMON" ] || exit 0
   
   case "$1" in
     start)
       echo "Starting $DESC:" "$NAME"
       $DAEMON $DAEMON_ARGS
     ;;
     stop)
       echo "Stopping $DESC:" "$NAME"
       $KILL $NAME
     ;;
     *)
       echo "Usage: $SCRIPTNAME {start|stop}" >&2
       exit 3
     ;;
   esac

証明書の準備
============

Stone用の証明書を準備する必要があります．ほんもののHTTPS通信と見せかけ
るためには，きちんと証明書を作っておく必要があります．

なお，OpenSSLの使い方は忘れやすいのでメモです．数年前までPKIの専門家
（仮）だった知識を活かして，一撃で自己署名証明書の発行方法を記します．

全部１行で
::

   # openssl req -new -x509 -days 3650 -newkey rsa:2048 -nodes \
   -keyout server.key -out server.crt -subj 'CN=cat.developlus.jp'

これで，「server.key」（パスフレーズなし）と「server.crt」が生成されま
す．

   
.. author:: default
.. categories:: Blog
.. tags:: stone
.. comments::
