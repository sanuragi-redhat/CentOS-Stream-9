  	
If you'd like to set Alias (Another Name) to a Host, set CNAME record in a zone file.

[1] 	Set CNAME record in a zone file. 

    [root@centos-stream9-n1 ~]# cat /var/named/forward.zone 
    $TTL 1D
    @	IN SOA	root.ocp2.redhat-workshop.in.  centos-stream9-n1.ocp2.redhat-workshop.in. (
    					0	; serial
    					1D	; refresh
    					1H	; retry
    					1W	; expire
    					3H )	; minimum
    @	IN	NS	centos-stream9-n1.ocp2.redhat-workshop.in.
    @	IN	A	192.168.1.156
    centos-stream9-n1.ocp2.redhat-workshop.in.	IN	A	192.168.1.156
    centos-stream9-n2.ocp2.redhat-workshop.in.	IN	A	192.168.1.157
    centos-stream9-n3.ocp2.redhat-workshop.in.	IN	A	192.168.1.158
    ; [Alias] IN CNAME centos-stream9-n1.ocp2.redhat-workshop.in.
    ftp 	IN	CNAME  centos-stream9-n1.ocp2.redhat-workshop.in.
    
    [root@centos-stream9-n1 ~]# rndc reload 
    server reload successful
    
    # verify resolution
    [root@centos-stream9-n1 ~]# dig ftp.ocp2.redhat-workshop.in.
    
    ; <<>> DiG 9.16.23-RH <<>> ftp.ocp2.redhat-workshop.in.
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 2317
    ;; flags: qr aa rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1
    
    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 1232
    ; COOKIE: b14603ba9925bd080100000068e058d269473b854f87ac08 (good)
    ;; QUESTION SECTION:
    ;ftp.ocp2.redhat-workshop.in.	IN	A
    
    ;; ANSWER SECTION:
    ftp.ocp2.redhat-workshop.in. 86400 IN	CNAME	centos-stream9-n1.ocp2.redhat-workshop.in.
    centos-stream9-n1.ocp2.redhat-workshop.in. 86400 IN A 192.168.1.156
    
    ;; Query time: 0 msec
    ;; SERVER: 192.168.1.156#53(192.168.1.156)
    ;; WHEN: Sat Oct 04 04:44:26 IST 2025
    ;; MSG SIZE  rcvd: 132
