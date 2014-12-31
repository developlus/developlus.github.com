================================
サーバリニューアルのすゝめ（３）
================================

Postfixの準備
=============

「カゴヤ・クラウド／VPS」でUbuntuを導入したところ，標準でSendmailが動
作していましたが，面倒なのですべてのSendmail関連パッケージを削除しまし
た．

まずは，インストール・・・

::

   # aptitude install postfix

とりあえず，Debianパッケージだとローカルのみだとか，イントラネットだと
か，メールサーバを配置する場所に応じて初期値となる設定値を埋めてくれま
す．とりあえず「インターネット上」に設定しておく．


main.cfの設定
-------------

何はなくとも設定ファイル「/etc/postfix/main.cf」をいじります．

::

   # クライアントに無駄な情報を教えない
   smtpd_banner = $myhostname ESMTP

   # IMAP経由でメールを読むので，Maildirに変更
   home_mailbox = Maildir/

   # SMPTS＋認証をやるので設定追加
   smtpd_sasl_auth_enable = yes
   smtpd_sasl_security_options = noanonymous
   smtpd_sasl_local_domain = $mydomain
   broken_sasl_auth_clients = yes
   smtpd_recipient_restrictions =
       permit_mynetworks,
       permit_sasl_authenticated,
       reject_unauth_destination,
       permit

   smtpd_client_restrictions =
       permit_mynetworks,
       check_client_access hash:/etc/postfix/access,
       # reject_rbl_client all.rbl.jp,
       reject_rbl_client bl.spamcop.net,
       # reject_rbl_client list.dsbl.org,
       # reject_rbl_client sbl-xbl.spamhaus.org,
       reject_unauth_destination,
       permit
       
   # RBLでスパム判定したら，レスポンスコードを550に設定
   maps_rbl_reject_code = 550
   default_rbl_reply = $rbl_code <$recipient>: Recipient address rejected: User unknown in local recipient tables

SASL2の設定
-----------

SMTPS認証を実施するために，SASL2を利用します．
幸い最初からインストールされていましたが，以下のパッケージが必要になり
ます．

::

   # aptitude install sasl2-bin sasl2-modules

これだけで，SASL2は動作しますが，chrootされたPostfixからでも認証結果
キャッシュにアクセスできるように設定することを基本になっているようで，
Postfixの参照先を変更するか，SASL2のワーキングディレクトリを変更するか，
どちらかを実施する必要があります．

とりあえず後者で設定するならば，SASL2のワーキングディレクトリを
「/var/spool/postfix/var/run/saslauthd」に吐き出すよう変更します．

::

   （ワーキングディレクトリの変更）
   OPTIONS="-c -m /var/spool/postfix/var/run/saslauthd"

さらに上記ワーキングディレクトリへPostfixアカウントから参照可能となる
よう，saslグループにpostfixアカウントを追加します．

::

   # usermod -a -G sasl postfix

master.cfの設定
---------------

SMTPSをListenするために，「/etc/postfix/master.cf」を変更します．
コメントアウトを外すだけ．

::

   submission inet n       -       -       -       -       smtpd
       -o syslog_name=postfix/submission
       -o smtpd_tls_security_level=encrypt
       -o smtpd_sasl_auth_enable=yes
       -o smtpd_client_restrictions=permit_sasl_authenticated,reject
       -o milter_macro_daemon_name=ORIGINATING
   smtps     inet  n       -       -       -       -       smtpd
       -o syslog_name=postfix/smtps
       -o smtpd_tls_wrappermode=yes
       -o smtpd_sasl_auth_enable=yes
       -o smtpd_client_restrictions=permit_sasl_authenticated,reject
       -o milter_macro_daemon_name=ORIGINATING

さらに，SASL2を参照して認証する場合の設定を
「/etc/postfix/sasl/smtpd.conf」に記載します．

::

   pwcheck_method: saslauthd
   mech_list: plain login

必要なサービスを再起動してください．

::

   # /etc/init.d/saslauthd restart
   # /etc/init.d/postfix restart


Dovecotの準備
=============

以前は何が何でもCourier-IMAPを利用していましたが，最近はDovecotを利用
するようになりました．

まずは，Dovecotのインストール

::

   # aptitude install dovecot-imapd dovecot-common

メールディレクトリ設定
----------------------

Courier-IMAPと同様に利用するためにはひと工夫が必要なので，設定ファイル
「/etc/dovecot/conf.d/10-mail.conf」を編集します．

::

   （メールディレクトリをMaildirに変更）
   mail_location = maildir:~/Maildir
   
   （Courier-IMAPと同じメールボックス設定にする）
   namespace inbox {
       separator = .
       prefix = INBOX.
       inbox = yes
   }

あとは，サービスを再起動しておけばOK．

::

   # /etc/init.d/dovecot restart

.. author:: default
.. categories:: Blog
.. tags:: vps, postfix
.. comments::
