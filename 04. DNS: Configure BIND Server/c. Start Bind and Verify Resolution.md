[1] 	Start and Enable BIND. 

    [root@centos-stream9-n1 ~]# systemctl enable --now named 
    Created symlink /etc/systemd/system/multi-user.target.wants/named.service → /usr/lib/systemd/system/named.service.
    

[2] 	If Firewalld is running, allow DNS service. DNS uses [53/TCP,UDP]. 

    [root@centos-stream9-n1 ~]# firewall-cmd --add-service=dns 
    [root@centos-stream9-n1 ~]# firewall-cmd --runtime-to-permanent 

    
[3] 	Change DNS setting to refer to own DNS if need.
(replace [enp1s0] to your own environment).

    [root@centos-stream9-n1 ~]# nmcli con show
    NAME    UUID                                  TYPE      DEVICE 
    enp1s0  767afc98-8e6d-3db8-9a15-7b22890fb0e8  ethernet  enp1s0 
    lo      4acfaf79-2aa3-4a1c-8614-c0b7abb5b6a9  loopback  lo  

    [root@centos-stream9-n1 ~]# nmcli connection modify enp1s0 ipv4.dns 192.168.1.156 
    [root@centos-stream9-n1 ~]# nmcli con down enp1s0 ; nmcli con up enp1s0 
    
[4] 	Verify Name and Address Resolution. If [ANSWER SECTION] is shown, that's OK. 

    [root@centos-stream9-n1 ~]# dig ocp2.redhat-workshop.in.
    
    ; <<>> DiG 9.16.23-RH <<>> ocp2.redhat-workshop.in.
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 8920
    ;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1
    
    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 1232
    ; COOKIE: a3bd04c43169a5040100000068e058459643bf5cd70e93d7 (good)
    ;; QUESTION SECTION:
    ;ocp2.redhat-workshop.in.	IN	A
    
    ;; ANSWER SECTION:
    ocp2.redhat-workshop.in. 86400	IN	A	192.168.1.156
    
    ;; Query time: 0 msec
    ;; SERVER: 192.168.1.156#53(192.168.1.156)
    ;; WHEN: Sat Oct 04 04:42:05 IST 2025
    ;; MSG SIZE  rcvd: 96
    
    [root@centos-stream9-n1 ~]# dig -x 192.168.1.156
    ; <<>> DiG 9.16.23-RH <<>> -x 192.168.1.156
    ;; global options: +cmd
    ;; Got answer:
    ;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 57928
    ;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1
    
    ;; OPT PSEUDOSECTION:
    ; EDNS: version: 0, flags:; udp: 1232
    ; COOKIE: 2670739eba162e5d0100000068e0585c24c94531ec60e7f5 (good)
    ;; QUESTION SECTION:
    ;156.1.168.192.in-addr.arpa.	IN	PTR
    
    ;; ANSWER SECTION:
    156.1.168.192.in-addr.arpa. 86400 IN	PTR	centos-stream9-n1.ocp2.redhat-workshop.in.
    
    ;; Query time: 0 msec
    ;; SERVER: 192.168.1.156#53(192.168.1.156)
    ;; WHEN: Sat Oct 04 04:42:28 IST 2025
    ;; MSG SIZE  rcvd: 138
    
