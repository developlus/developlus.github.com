==================================
 サーバリニューアルのすゝめ（５）
==================================

IPv6無効化
==========

以外とハマりやすいのがIPv6関連．ローカルにHTTP Proxy（Squid）をたてて，
プロキシサーバにしようとしたところ，設定は間違っていないのになぜかWeb
アクセスできない・・・と悩んで，ログを確認してみると，DIRECTアクセス先
の指定がIPv6になっている．しかし，IPv6 Readyな環境ではないのでもちろん
タイムアウトします．

そんなときは，IPv6を利用しないように設定します．

設定は簡単「/etc/sysctl.conf」の末尾あたりに設定を追加．

::

   net.ipv6.conf.all.disable_ipv6 = 1
   net.ipv6.conf.default.disable_ipv6 = 1
   net.ipv4.tcp_syncookies=1
   net.ipv4.icmp_echo_ignore_broadcasts=1
   net.ipv4.conf.all.accept_redirects=0
   net.ipv4.conf.default.accept_redirects=0
   net.ipv4.conf.lo.accept_redirects=0
   net.ipv4.conf.venet0.accept_redirects=0
   net.ipv4.conf.all.accept_source_route=0
   net.ipv4.conf.default.accept_source_route=0
   net.ipv4.conf.lo.accept_source_route=0
   net.ipv4.conf.venet0.accept_source_route=0

まぁ，関係無さそうな設定も含まれていますが，ちなみにvenet0はVPSのNICで
す．


.. author:: default
.. categories:: Blog
.. tags:: vps
.. comments::
