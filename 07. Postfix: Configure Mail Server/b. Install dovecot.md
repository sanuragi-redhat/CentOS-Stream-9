## Mail Server : Install Dovecot
 	
### Install Dovecot to configure POP/IMAP Server.

Install Dovecot. 

    [root@centos-stream9-n1 ~]# vi /etc/dovecot/dovecot.conf 
    
    # line 30 : uncomment (if not use IPv6, remove [::])
    listen = *, :: 

This example shows to configure to provide SASL function to Postfix. 
 
   [root@centos-stream9-n1 ~]# vi /etc/dovecot/conf.d/10-auth.conf 
    # line 10 : uncomment and change (allow plain text auth)
    disable_plaintext_auth = no
    # line 100 : add
    auth_mechanisms = plain login
    
    [root@centos-stream9-n1 ~]#  vi /etc/dovecot/conf.d/10-mail.conf 
    # line 30 : uncomment and add
    mail_location = maildir:~/Maildir
            
    [root@centos-stream9-n1 ~]# vi /etc/dovecot/conf.d/10-master.conf 
    # line 107-109 : uncomment and add like follows
      # Postfix smtp-auth
      unix_listener /var/spool/postfix/private/auth {
        mode = 0666
        user = postfix
        group = postfix
      }
    
    [root@centos-stream9-n1 ~]#  vi /etc/dovecot/conf.d/10-ssl.conf 
    # line 8 : change (not require SSL)
    ssl = yes

    [root@centos-stream9-n1 ~]# systemctl enable --now dovecot 

If Firewalld is running, allow POP/IMAP service. POP uses [110/TCP], IMAP uses [143/TCP].

    [root@centos-stream9-n1 ~]# firewall-cmd --add-service={pop3,imap}
    success
    [root@centos-stream9-n1 ~]# firewall-cmd --runtime-to-permanent
    success 
