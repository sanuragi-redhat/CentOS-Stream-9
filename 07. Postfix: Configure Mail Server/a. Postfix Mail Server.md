## Mail Server : Install Postfix
 	
### Install Postfix to configure SMTP Server

Install Postfix. 

    [root@centos-stream9-n1 ~]# dnf -y install postfix 
    

This example shows to configure SMTP-Auth settings to use Dovecot's SASL feature. 

    [root@centos-stream9-n1 ~]# vi /etc/postfix/main.cf
    # line 95 : uncomment and specify hostname
    myhostname = mail.ocp2.redhat-workshop.in
    
    # line 102 : uncomment and specify domain name
    mydomain = ocp2.redhat-workshop.in
    
    # line 118 : uncomment
    myorigin = $mydomain
    
    # line 135 : change
    inet_interfaces = all
    
    # line 138 : change it if use only IPv4
    inet_protocols = ipv4
    
    # line 183 : add
    mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain
    
    # line 283 : uncomment and specify your local network
    mynetworks = 127.0.0.0/8, 192.168.1.0/24
    
    # line 438 : uncomment (use Maildir)
    home_mailbox = Maildir/
    
    # line 593 : add
    # hide the kind or version of SMTP software
    smtpd_banner = $myhostname ESMTP
    
    # add follows to last line
    # disable SMTP VRFY command
    disable_vrfy_command = yes
    
    # require HELO command to sender hosts
    smtpd_helo_required = yes
    
    # limit an email size
    # example below means 10M bytes limit
    message_size_limit = 10240000
    
    # SMTP-Auth settings
    smtpd_sasl_type = dovecot
    smtpd_sasl_path = private/auth
    smtpd_sasl_auth_enable = yes
    smtpd_sasl_security_options = noanonymous
    smtpd_sasl_local_domain = $myhostname
    smtpd_recipient_restrictions = 
      permit_mynetworks,
      permit_sasl_authenticated,
      reject_unauth_destination
    
    [root@centos-stream9-n1 ~]# systemctl enable --now postfix 
    Created symlink /etc/systemd/system/multi-user.target.wants/postfix.service → /usr/lib/systemd/system/postfix.service.

If Firewalld is running, allow SMTP service. SMTP uses [25/TCP]. 

    [root@centos-stream9-n1 ~]# firewall-cmd --permanent --add-port=25/tcp 
    [root@centos-stream9-n1 ~]# firewall-cmd --reload 

Configure additional settings for Postfix if you need.
    It's possible to reject many spam emails with the settings below.
    However, you should consider to apply the settings,
    because sometimes normal emails are also rejected with them.
    Especially, there are SMTP servers that forward lookup and reverse lookup of their hostnames on DNS do not match even if they are not spammers. 


    [root@centos-stream9-n1 ~]# vi /etc/postfix/main.cf
    
    # add to the end
    # reject unknown clients that forward lookup and reverse lookup of their hostnames on DNS do not match
    smtpd_client_restrictions = permit_mynetworks, reject_unknown_client_hostname, permit
    
    # rejects senders that domain name set in FROM are not registered in DNS or 
    # not registered with FQDN
    smtpd_sender_restrictions = permit_mynetworks, reject_unknown_sender_domain, reject_non_fqdn_sender
    
    # reject hosts that domain name set in FROM are not registered in DNS or 
    # not registered with FQDN when your SMTP server receives HELO command
    smtpd_helo_restrictions = permit_mynetworks, reject_unknown_hostname, reject_non_fqdn_hostname, reject_invalid_hostname, permit
    
    [root@centos-stream9-n1 ~]# systemctl restart postfix 