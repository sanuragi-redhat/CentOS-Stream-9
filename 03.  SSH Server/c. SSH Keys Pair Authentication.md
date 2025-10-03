  	
## Configure SSH server to login with Key-Pair Authentication.

Create a private key for client and a public key for server to do it.

[1] 	Create Key-Pair by each user, so login with a common user on SSH Server Host and work like follows.

    # create key-pair
    [redhat@centos-stream9-n1 ~]$ ssh-keygen
    Generating public/private ed25519 key pair.
    Enter file in which to save the key (/home/redhat/.ssh/id_ed25519): 
    Created directory '/home/redhat/.ssh'.
    Enter passphrase for "/home/redhat/.ssh/id_ed25519" (empty for no passphrase): 
    Enter same passphrase again: 
    Your identification has been saved in /home/redhat/.ssh/id_ed25519
    Your public key has been saved in /home/redhat/.ssh/id_ed25519.pub
    The key fingerprint is:
    SHA256:6oxHR3YzPvDbrYHbbRoeFS3HWRqsjhmuvRMaFhnDWTE redhat@centos-stream9-n1.ocp2.redhat-workshop.in
    The key's randomart image is:

    .....
    .....
    
    [redhat@centos-stream9-n1 ~]$ ll ~/.ssh
    total 8
    -rw-------. 1 redhat redhat 452 Oct  4 02:47 id_ed25519
    -rw-r--r--. 1 redhat redhat 130 Oct  4 02:47 id_ed25519.pub
    
 

[2] 	Transfer the public key  created on the Server to a Client, then it's possible to login with Key-Pair authentication.
    
    [redhat@centos-stream9-n2 ~]$ mkdir ~/.ssh
    [redhat@centos-stream9-n2 ~]$ chmod 700 ~/.ssh
    [redhat@centos-stream9-n2 ~]$ touch ~/.ssh/authorized_keys
    
    # transfer the public key to client machine.
    [redhat@centos-stream9-n1 ~]$ scp -r ~/.ssh/id_ed25519.pub redhat@centos-stream9-n2:~/.ssh/authorized_keys 
    The authenticity of host 'centos-stream9-n2 (192.168.1.157)' can't be established.
    ED25519 key fingerprint is SHA256:GqerXj8IymGlujYdbW9E2dys5apMIXiGYiVmUusuaSM.
    This key is not known by any other names.
    Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
    Warning: Permanently added 'centos-stream9-n2' (ED25519) to the list of known hosts.
    redhat@centos-stream9-n2's password: 
    id_ed25519.pub                                100%  130   742.6KB/s   00:00 
        
    [redhat@centos-stream9-n1 ~]$ ssh redhat@centos-stream9-n2 
    Last login: Sat Oct  4 02:49:43 2025
    [redhat@centos-stream9-n2 ~]$ 
    

[3] 	If you set [PasswordAuthentication no], it's more secure.

    [root@centos-stream9-n1 ~]# vi /etc/ssh/sshd_config
    # line 65 : change to [no]
    PasswordAuthentication no
    # line 69 : if it's enabled, change it to [no], too
    
    # Change to no to disable s/key passwords
    #KbdInteractiveAuthentication yes
    KbdInteractiveAuthentication no
    [root@centos-stream9-n1 ~]# systemctl restart sshd 