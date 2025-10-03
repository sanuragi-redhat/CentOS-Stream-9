## OpenSSH: SFTP only + Chroot

Configure SFTP only + Chroot.
Some users who are applied this setting can access only with SFTP and also applied chroot directory. 

[1] 	For example, Set [/home] as the Chroot directory. 
    
     # create a group for SFTP only
    [root@centos-stream9-n1 ~]# groupadd sftp_users
    
    [root@centos-stream9-n1 ~]# vi /etc/ssh/sshd_config
    # line 123 : comment out and add a line
    #Subsystem      sftp    /usr/libexec/openssh/sftp-server
    Subsystem       sftp    internal-sftp
    
    # add to the end
    Match Group sftp_users
      X11Forwarding no
      AllowTcpForwarding no
      ChrootDirectory /home
      ForceCommand internal-sftp
    [root@centos-stream9-n1 ~]# systemctl restart sshd

# for example, set [cent] user as SFTP only user
[root@centos-stream9-n1 ~]# usermod -aG sftp_users cent 

[2] 	Verify working with a user set SFTP only setting.

    [redhat@centos-stream9-n3 ~]$ ssh centos-stream9-n1
    redhat@centos-stream9-n1's password: 
    This service allows sftp connections only.
    Connection to centos-stream9-n1 closed.   # denied normally

    [redhat@centos-stream9-n3 ~]$ sftp centos-stream9-n1
    redhat@centos-stream9-n1's password: 
    Connected to centos-stream9-n1.
    sftp> ls -l 
    drwx------    ? 1000     1000           95 Oct  4 02:58 redhat
    sftp> pwd
    Remote working directory: /
    sftp> exit
    