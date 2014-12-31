==================
 スパムメール対策
==================

サーバをリニューアルしたのは良いものの，メールサーバの設定を手抜きして
しまった．RBLを沢山登録しておいたら，Twitterからのメールすら届かない始
末．

手作業でチェックした結果，以下のRBL設定のみ残すこととしました．

::

   smtpd_client_restrictions =
        permit_mynetworks,
        check_client_access hash:/etc/postfix/access,
        #reject_rbl_client all.rbl.jp,
        reject_rbl_client bl.spamcop.net,
        reject_rbl_client sbl-xbl.spamhaus.org,
        reject_rbl_client xbl.spamhaus.org,
        reject_rbl_client cbl.abuseat.org,
        reject_rbl_client psbl.surriel.com,
        reject_unauth_destination,
        permit

だいぶ少なくなってしまった．

これでもスパム送信をやめてくれないホストは，そもそもSMTP Connectを拒否っ
てしまいます．
ただ，手探りでスパム送信元を探すのも面倒なので，メールログを解析してく
れるツールを導入します．Postfixのログといえば，「pflogsumm」です．

サクっとインストールして，Cronに登録します．

::

   # aptitude install pflogsumm
   # vim /etc/cron.daily/pflogsumm
   
   #!/bin/sh
   /usr/bin/perl /usr/sbin/pflogsumm --verbose_msg_detail -e -d
   yesterday /var/log/mail.log | mail -s 'Logwatch for Postfix' root

   # chmod +x /etc/cron.daily/pflogsumm


以上です．

あとは，毎日メールログ解析結果がメールで届くので，「message reject
detail」に記載されているログにおいて，明らかにスパム（変なドメイン，海
外ドメイン）の送信元IPを「/etc/postfix/access」でREJECTしてしまう．

main.cfに以下のような設定が必要です．

::

   smtp_client_restrictions =
       check_client_access hash:/etc/postfix/access

さらに，accessファイルはハッシュ化しておく必要があるので．

::

   # vim /etc/postfix/access
   （拒否しているIP自分で確認してください）
   203.190.62.96   REJECT
   182.48.33.218   REJECT
   163.43.162.90   REJECT
   110.165.4.16    REJECT
   97.88.229.119   REJECT
   87.246.193.114  REJECT
   
   # postmap /etc/postfix/access

こんな感じです．

.. author:: default
.. categories:: Blog
.. tags:: postfix
.. comments::
