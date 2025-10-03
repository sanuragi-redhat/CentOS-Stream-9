### OpenSSH : Use SSHFS

[1] Install fuse-sshfs.

    # install from EPEL
    [root@centos-stream9-n1 ~]# yum install epel-release -y 
    [root@centos-stream9-n1 ~]#  dnf --enablerepo=epel -y install fuse fuse-sshfs 

[2] 	It's possible to use by common users.
For example, [redhat] user mount [/home/redhat/work] on [centos-stream9-n1] to local [~/sshmnt]. 

    [redhat@centos-stream9-n1 ~]$ mkdir ~/sshmnt
    
    # mount with SSHFS
    [redhat@centos-stream9-n1 ~]$ sshfs centos-stream9-n1:/home/redhat/work ~/sshmnt
    # Enter password
    redhat@centos-stream9-n1's password: 

    [redhat@centos-stream9-n1 ~]$ df -hT
    Filesystem                          Type        Size  Used Avail Use% Mounted on
    devtmpfs                            devtmpfs    4.0M     0  4.0M   0% /dev
    tmpfs                               tmpfs       854M     0  854M   0% /dev/shm
    tmpfs                               tmpfs       342M  4.9M  337M   2% /run
    /dev/mapper/cs-r    oot                 xfs          17G  1.7G   16G  10% /
    /dev/vda1                           xfs         960M  251M  710M  27% /boot
    tmpfs                               tmpfs       171M     0  171M   0% /run/user/0
    centos-stream9-n1:/home/redhat/work fuse.sshfs   17G  1.7G   16G  10% /home/redhat/sshmnt
    tmpfs                               tmpfs       171M     0  171M   0% /run/user/1000
    
    
    [redhat@centos-stream9-n1 ~]$ fusermount -u ~/sshmnt 