 	
Use SSHPass to automate inputting password on password authentication.
This is convenient but it has security risks (leak of password), take special care if you use it.

[1] 	Install SSHPass.

    [root@centos-stream9-n1 ~]# yum install sshpass -y 
    
[2] 	How to use SSHPass.
    
    # [-p password] : from argument
    [redhat@centos-stream9-n1 ~]$ sshpass -p password ssh centos-stream9-n2
    Last login: Sat Oct  4 03:06:36 2025 from 192.168.1.156
    [redhat@centos-stream9-n2 ~]$ exit
    logout
    Connection to centos-stream9-n2 closed.

    [redhat@centos-stream9-n1 ~]$ sshpass -p password ssh root@centos-stream9-n2
    Last login: Sat Oct  4 03:07:01 2025 from 192.168.1.25
    [root@centos-stream9-n2 ~]# exit
    Connection to centos-stream9-n2 closed. 
    
    # [-f file] : from file
    [redhat@centos-stream9-n1 ~]$ echo 'password' > sshpass.txt

    [redhat@centos-stream9-n1 ~]$ chmod 600 sshpass.txt 
        
    [redhat@centos-stream9-n1 ~]$ sshpass -f sshpass.txt ssh centos-stream9-n2 hostname
    centos-stream9-n2.ocp2.redhat-workshop.in
    
    # [-e] : from environment variable
    [redhat@centos-stream9-n1 ~]$  export SSHPASS=password
        
    [redhat@centos-stream9-n1 ~]$ sshpass -e ssh centos-stream9-n2 hostname
    centos-stream9-n2.ocp2.redhat-workshop.in
    