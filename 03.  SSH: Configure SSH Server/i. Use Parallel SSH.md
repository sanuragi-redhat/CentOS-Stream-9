### OpenSSH : Use Parallel SSH

Use PSSH (Parallel Secure Shell) to connect to multiple hosts via SSH. 

[1] 	Install PSSH.     
    
    [root@centos-stream9-n1 ~]# dnf --enablerepo=epel -y install pssh 
    
[2] Basic usage for PSSH.
This is based on the environment you set SSH key-pair to target Hosts without passphrase.
If set passphrase, run SSH-Agent and set passphrase on it first. 

    # specify target hosts and access via SSH
    [redhat@centos-stream9-n1 ~]$ pssh -H "192.168.1.157 192.168.1.158" -i "hostname" 
    [1] 03:25:29 [SUCCESS] 192.168.1.157
    centos-stream9-n2.ocp2.redhat-workshop.in
    [2] 03:25:29 [SUCCESS] 192.168.1.158
    centos-stream9-n3.ocp2.redhat-workshop.in
    
    [redhat@centos-stream9-n1 ~]$ cat pssh_hosts.txt 
    redhat@centos-stream9-n2.ocp2.redhat-workshop.in
    redhat@centos-stream9-n3.ocp2.redhat-workshop.in
    
    [redhat@centos-stream9-n1 ~]$ pssh -h pssh_hosts.txt -i "uptime" 
    [1] 03:27:02 [SUCCESS] redhat@centos-stream9-n3.ocp2.redhat-workshop.in
     03:27:02 up  4:01,  0 users,  load average: 0.00, 0.00, 0.00
    [2] 03:27:02 [SUCCESS] redhat@centos-stream9-n2.ocp2.redhat-workshop.in
     03:27:02 up  4:01,  1 user,  load average: 0.00, 0.00, 0.00
    
[3] It's possible to connect with password authentication too, but it needs passwords on all hosts are the same one. 

    [redhat@centos-stream9-n1 ~]$ pssh -h pssh_hosts.txt -A -O PreferredAuthentications=password -i "uname -r" 
    Warning: do not enter your password if anyone else has superuser
    privileges or access to your account.
    Password: # input password
    [1] 03:27:29 [SUCCESS] redhat@centos-stream9-n3.ocp2.redhat-workshop.in
    5.14.0-620.el9.x86_64
    [2] 03:27:29 [SUCCESS] redhat@centos-stream9-n2.ocp2.redhat-workshop.in
    5.14.0-620.el9.x86_64

[4] PSSH package includes pscp.pssh, prsync, pslurp, pnuke commands and you can use them with the same usage of pssh. 