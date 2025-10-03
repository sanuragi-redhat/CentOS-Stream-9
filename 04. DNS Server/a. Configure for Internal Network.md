## BIND: Configuration 

### Configure DNS Server

Install DNS Package.

    [root@centos-stream9-n1 ~]# yum install bind bind-utils -y 
    
Configure BIND for Internal Network.   

    [root@centos-stream9-n1 ~]#  vi /etc/named.conf 
    ...
    ...
    options {
            # change  ( listen ip )
            listen-on port 53 { 192.168.1.156; };
            # change if need  if not listen IPv6, set [none] )
            listen-on-v6 port 53 { any; };
            directory       "/var/named";
            dump-file       "/var/named/data/cache_dump.db";
            statistics-file "/var/named/data/named_stats.txt";
            memstatistics-file "/var/named/data/named_mem_stats.txt";
            secroots-file   "/var/named/data/named.secroots";
            recursing-file  "/var/named/data/named.recursing";
            allow-query     { 192.168.1.0/24; };
    ...
    ...
    zone "ocp2.redhat-workshop.in" IN {
            type master;
            file "forward.zone";
    };
    zone "1.168.192.in-addr.arpa" IN {
            type master;
            file "reverse.zone";
    };

If you don't use IPv6 and also suppress logs for IPv6 related, possible to change

    # set BIND to use only IPv4
    [root@centos-stream9-n1 ~]# vi /etc/sysconfig/named
    # add to the end   
    OPTIONS="-4"
    
