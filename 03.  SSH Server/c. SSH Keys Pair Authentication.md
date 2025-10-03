  	
## Configure SSH server to login with Key-Pair Authentication.

Create a private key for client and a public key for server to do it.

[1] 	Create Key-Pair by each user, so login with a common user on SSH Server Host and work like follows.

    # create key-pair
    [redhat@centos-stream9-n1 ~]$ ssh-keygen
    
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/cent/.ssh/id_rsa):   # Enter or input changes if you want
    Created directory '/home/cent/.ssh'.
    Enter passphrase (empty for no passphrase):   # set passphrase (if set no passphrase, Enter with empty)
    Enter same passphrase again:
    Your identification has been saved in /home/cent/.ssh/id_rsa
    Your public key has been saved in /home/cent/.ssh/id_rsa.pub
    The key fingerprint is:
    SHA256:yYOKaIcT25Jd0ZaOOYLa+rgrU0c/M/rVmJx4q4MVZB0 cent@centos-stream9-n1.ocp2.redhat-workshop.in
    The key's randomart image is:
    .....
    .....
    
    [redhat@centos-stream9-n1 ~]$ ll ~/.ssh
    
    total 8
    -rw-------. 1 cent cent 2655 Jan  7 19:07 id_rsa
    -rw-r--r--. 1 cent cent  575 Jan  7 19:07 id_rsa.pub
    
    [redhat@centos-stream9-n1 ~]$ mv ~/.ssh/id_rsa.pub ~/.ssh/authorized_keys

[2] 	Transfer the private key created on the Server to a Client, then it's possible to login with Key-Pair authentication.
    [cent@centos-stream9-n1 ~]$ mkdir ~/.ssh
    
    [cent@centos-stream9-n1 ~]$ chmod 700 ~/.ssh
# transfer the private key to the local ssh directory

    [cent@centos-stream9-n1 ~]$ scp redhat@centos-stream9-n1.ocp2.redhat-workshop.in:/home/cent/.ssh/id_rsa ~/.ssh/
    
    redhat@centos-stream9-n1.ocp2.redhat-workshop.in's password:
    id_rsa                                        100% 1876   193.2KB/s   00:00
    
    [cent@centos-stream9-n1 ~]$ ssh redhat@centos-stream9-n1.ocp2.redhat-workshop.in
    
Enter passphrase for key '/home/cent/.ssh/id_rsa':   # passphrase if you set

    [redhat@centos-stream9-n1 ~]$   # logined

[3] 	If you set [PasswordAuthentication no], it's more secure.

    [root@dlp ~]# vi /etc/ssh/sshd_config
    # line 65 : change to [no]
    
    PasswordAuthentication no
    # line 69 : if it's enabled, change it to [no], too
    
    # Change to no to disable s/key passwords
    #KbdInteractiveAuthentication yes
    KbdInteractiveAuthentication no
    [root@dlp ~]# systemctl restart sshd 