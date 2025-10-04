### ISCSI Server.  	

Configure Storage Server with iSCSI.

Storage server with iSCSI on network is called iSCSI Target, Client Host that connects to iSCSI Target is called iSCSI Initiator.

This example is based on the environment like follows.
    
    +-----------------------+              |              +----------------------+
    | [   iSCSI Target   ]  |192.168.1.156 | 192.168.1.157| [ iSCSI Initiator  ] |
    |     centos-stream9-n1 +--------------+--------------+   centos-stream9-n2  |
    |                       |              |                                     |
    +-----------------------+              +-------------------------------------+
    
### [1] Install administration tool. 

    [root@centos-stream9-n1 ~]# dnf -y install targetcli 
    
    
### [2] Configure iSCSI Target.

    # create a directory
    [root@centos-stream9-n1 ~]# mkdir /var/lib/iscsi_disk

### [3] enter the admin console
    
    [root@centos-stream9-n1 ~]# targetcli
    
    targetcli shell version 2.1.53
    Copyright 2011-2013 by Datera, Inc and others.
    For help on commands, type 'help'.
    
    /> cd /backstores/fileio 
    
    # create a disk-image with the name [disk01] on [/var/lib/iscsi_disks/disk01.img] with 10G
    /backstores/fileio> create disk01 /var/lib/iscsi_disks/disk01.img 5G 
    Created fileio disk01 with size 5368709120
    
    /backstores/fileio> cd /iscsi 
    
    # create a target
    # naming rule : [ iqn.(year)-(month).(reverse of domain name):(any name you like) ]
     /iscsi> create iqn.2025-09.redhat-workshop.centos-stream9:n1.target01 
    Created target iqn.2025-09.redhat-workshop.centos-stream9:n1.target01.
    Created TPG 1.
    Global pref auto_add_default_portal=true
    Created default portal listening on all IPs (0.0.0.0), port 3260.

    /iscsi> cd iqn.2025-09.redhat-workshop.centos-stream9:n1.target01/tpg1 
    
    # enable authentication
    /iscsi/iqn.20...target01/tpg1> set attribute authentication=1 
    Parameter authentication is now '1'.
    /iscsi/iqn.20...target01/tpg1> cd luns 
    
    # set LUN
    /iscsi/iqn.20...t01/tpg1/luns> create /backstores/fileio/disk01 
    Created LUN 0.
    /iscsi/iqn.20...t01/tpg1/luns> cd ../acls 
    
    # set ACL (it's the IQN of an initiator you permit to connect)
    /iscsi/iqn.20...t01/tpg1/acls> create iqn.2025-09.redhat-workshop.centos-stream9:n1.initiator01 
    Created Node ACL for iqn.2025-09.redhat-workshop.centos-stream9:n1.initiator01
    Created mapped LUN 0.
    /iscsi/iqn.20...t01/tpg1/acls> cd iqn.2025-09.redhat-workshop.centos-stream9:n1.initiator01 
    
    # set UserID and Password for authentication
    /iscsi/iqn.20...w.initiator01> set auth userid=admin
    Parameter userid is now 'username'.
    /iscsi/iqn.20...w.initiator01> set auth password=password 
    Parameter password is now 'password'.
    /iscsi/iqn.20...w.initiator01> exit 
    Global pref auto_save_on_exit=true
    Last 10 configs saved in /etc/target/backup/.
    Configuration saved to /etc/target/saveconfig.json
    
    
    # after configuration above, the target enters in listening like follows
    
    [root@centos-stream9-n1 ~]# ss -napt | grep 3260
    LISTEN 0      256          0.0.0.0:3260       0.0.0.0:*
    
    [root@centos-stream9-n1 ~]# systemctl enable target 
    Created symlink /etc/systemd/system/multi-user.target.wants/target.service → /usr/lib/systemd/system/target.service.
