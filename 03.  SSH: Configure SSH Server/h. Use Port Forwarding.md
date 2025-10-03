### OpenSSH : SSH Port Forwarding

 It's possible to forward a port to another port with SSH port forwarding. 

[1]  For example, set SSH Port Forwarding that requests to port [8081] on [centos-stream9-n1 (192.168.1.156)] are forwarded to port [80] on [centos-stream9-n2 (192.168.1.157)]. 

    [root@centos-stream9-n1 ~]# ssh -L 192.168.1.156:8081:193.168.1.157:80 redhat@centos-stream9-n2 
    The authenticity of host 'centos-stream9-n2 (192.168.1.157)' can't be established.
    ED25519 key fingerprint is SHA256:GqerXj8IymGlujYdbW9E2dys5apMIXiGYiVmUusuaSM.
    This host key is known by the following other names/addresses:
        ~/.ssh/known_hosts:1: 192.168.1.157
    Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
    Warning: Permanently added 'centos-stream9-n2' (ED25519) to the list of known hosts.
    redhat@centos-stream9-n2's password: 
    Last login: Sat Oct  4 03:09:49 2025 from 192.168.1.156
    [redhat@centos-stream9-n2 ~]$ 
    
    # confirm state
    [redhat@centos-stream9-n2 ~]$ ssh  centos-stream9-n1 "ss -napt | grep 8081" 
    redhat@centos-stream9-n1's password: 
    LISTEN 0      128    192.168.1.156:8081        0.0.0.0:*   
    
    # listen on 8081
    # keep this login session

