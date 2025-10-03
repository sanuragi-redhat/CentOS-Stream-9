##  Configure SSH Server to operate servers from remote computers. 

[1] OpenSSH is already installed by default even if you installed CentOS Stream with [Minimal] Install, so it does not need to install new packages.
You can login with Password Authentication by default.
If you like to improve the security, you should change PermitRootLogin parameter.

    [root@centos-stream9-n1 ~]# vi /etc/ssh/sshd_config
    # line 40: change ( prohibit root login )
    # for other options, there are [prohibit-password], [forced-commands-only]
    PermitRootLogin no

    [root@centos-stream9-n1 ~]# systemctl restart sshd 

[2] If Firewalld is running, allow SSH service. SSH uses [22/TCP].
    [root@centos-stream9-n1 ~]# firewall-cmd --add-service=ssh
    success

    [root@centos-stream9-n1 ~]# firewall-cmd --runtime-to-permanent    
    success 

### SSH Client : CentOS

[3] 	Install SSH Client.

    [root@centos-stream9-n2 ~]# dnf -y install openssh-clients 
    
[4] 	Connect to SSH server with any common user. 

    [root@centos-stream9-n2 ~]# ssh redhat@centos-stream9-n1
    The authenticity of host 'centos-stream9-n1 (192.168.1.156)' can't be established.
    ED25519 key fingerprint is SHA256:P7soeogJNp2fbLeb8OgjwxpPgaOWVyAvKGc/jLZSm6M.
    This key is not known by any other names.
    Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
    Warning: Permanently added 'centos-stream9-n1' (ED25519) to the list of known hosts.
    redhat@centos-stream9-n1's password: 
    [redhat@centos-stream9-n1 ~]$  #logged-in

[5] 	It's possible to execute commands on remote Host with SSH like follows.

# for example, run [cat /etc/passwd]
    [redhat@centos-stream9-n2 ~]$ ssh redhat@centos-stream9-n2 "cat /etc/passwd"
    redhat@centos-stream9-n2's password: password
    root:x:0:0:root:/root:/bin/bash
    bin:x:1:1:bin:/bin:/sbin/nologin
    daemon:x:2:2:daemon:/sbin:/sbin/nologin
    adm:x:3:4:adm:/var/adm:/sbin/nologin
    lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
    .....
    .....
    systemd-oom:x:984:984:systemd Userspace OOM Killer:/:/usr/sbin/nologin
    systemd-resolve:x:983:983:systemd Resolver:/:/usr/sbin/nologin
    redhat:x:1000:1000::/home/redhat:/bin/bash
    
