
[1] Create zone files that servers resolve IP address from Domain name.
The example below uses Internal network [192.168.1.0/24], Domain name [ocp2.redhat-workshop.in].
Replace to your own environment. 

    [root@centos-stream9-n1 ~]# cd /var/named/
    [root@centos-stream9-n1 named]# cat forward.zone 
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
    
[2]	Create zone files that servers resolve Domain name from IP address.
The example below uses Internal network [192.168.1.0/24], Domain name [ocp2.redhat-workshop.in].

    [root@centos-stream9-n1 named]# cat reverse.zone 
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
    156	IN	PTR	centos-stream9-n1.ocp2.redhat-workshop.in.
    157	IN	PTR	centos-stream9-n2.ocp2.redhat-workshop.in.
    158	IN	PTR	centos-stream9-n3.ocp2.redhat-workshop.in.
    
[3] Change ownership for zone files. 

    [root@centos-stream9-n1 named]# chown root:named forward.zone 
    [root@centos-stream9-n1 named]# chown root:named reverse.zone 
    [root@centos-stream9-n1 named]# ll forward.zone reverse.zone 
    -rw-r-----. 1 root named 438 Oct  4 04:36 forward.zone
    -rw-r-----. 1 root named 600 Oct  4 04:36 reverse.zone
    
    
