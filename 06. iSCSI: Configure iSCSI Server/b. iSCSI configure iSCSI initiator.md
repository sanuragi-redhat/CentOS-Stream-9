### Configure iSCSI Initiator. 


This example is based on the environment like follows.
    
    +-----------------------+              |              +----------------------+
    | [   iSCSI Target   ]  |192.168.1.156 | 192.168.1.157| [ iSCSI Initiator  ] |
    |     centos-stream9-n1 +--------------+--------------+   centos-stream9-n2  |
    |                       |              |                                     |
    +-----------------------+              +-------------------------------------+

### [1]	Configure iSCSI Initiator to connect to iSCSI Target. 

Install iscsi-initiator-utils package. 

    [root@centos-stream9-n2 ~]# dnf -y install iscsi-initiator-utils 

Start and Enable iscsid service. 
    
    [root@centos-stream9-n2 ~]# systemctl enable --now iscsid
    
Make entry in /etc/iscsi/initiatorname.iscsi

    [root@centos-stream9-n2 ~]# vi /etc/iscsi/initiatorname.iscsi 
    InitiatorName=iqn.2025-09.redhat-workshop.centos-stream9:n1.initiator01

Configure Initiator utils. 
    
    [root@centos-stream9-n2 ~]# vi /etc/iscsi/iscsid.conf 
    # line 74 : uncomment
    node.session.auth.authmethod = CHAP
    # line 86,86 : uncomment and add credentials.
    node.session.auth.username = admin 
    node.session.auth.password = password

Discovery LUN. 
    
    [root@centos-stream9-n2 ~]# iscsiadm -m discovery -t sendtargets -p 192.168.1.156
    192.168.1.156:3260,1 iqn.2025-09.redhat-workshop.centos-stream9:n1.target01

Confirm status after discovery

    [root@node01 ~]# iscsiadm -m node -o show 
    # BEGIN RECORD 6.2.1.11
    node.name = iqn.2025-09.redhat-workshop.centos-stream9:n1.target01
    node.tpgt = 1
    node.startup = automatic
    node.leading_login = No
    iface.iscsi_ifacename = default
    ...
    ...
    node.conn[0].iscsi.DataDigest = None
    node.conn[0].iscsi.IFMarker = No
    node.conn[0].iscsi.OFMarker = No
    # END RECORD
    
Login to the target

    [root@centos-stream9-n2 ~]# iscsiadm -m node --login -T  iqn.2025-09.redhat-workshop.centos-stream9:n1.target01 -p 192.168.1.156 
    Login to [iface: default, target: iqn.2025-09.redhat-workshop.centos-stream9:n1.target01, portal: 192.168.1.156,3260] successful.

Enable Autologin for the Node

    [root@centos-stream9-n2 ~]# iscsiadm -m node -T iqn.2025-09.redhat-workshop.centos-stream9:n1.target01 -p 192.168.1.156 --op update -n node.startup -v automatic
    [root@centos-stream9-n2 ~]# iscsiadm -m node -T iqn.2025-09.redhat-workshop.centos-stream9:n1.target01 -p 192.168.1.156  --op update -n node.conn[0].startup -v automatic

To confirm that autologin is enabled:

    [root@centos-stream9-n2 ~]# iscsiadm -m node -T iqn.2025-09.redhat-workshop.centos-stream9:n1.target01 -p 192.168.1.156| grep startup
    node.startup = automatic
    node.conn[0].startup = automatic



Confirm the established session

    [root@centos-stream9-n2 ~]# iscsiadm -m session -o show 
    tcp: [2] 192.168.1.156:3260,1 iqn.2025-09.redhat-workshop.centos-stream9:n1.target01 (non-flash)
    
Listing a disk.  

    [root@centos-stream9-n2 ~]# lsblk
    NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
    sda           8:0    0    5G  0 disk 
    sr0          11:0    1 1024M  0 rom  
    vda         252:0    0   20G  0 disk 
    ├─vda1      252:1    0    1G  0 part /boot
    └─vda2      252:2    0   19G  0 part 
      ├─cs-root 253:0    0   17G  0 lvm  /
      └─cs-swap 253:1    0    2G  0 lvm  [SWAP]

Format and Use this disk. 

    [root@centos-stream9-n2 ~]# parted --script /dev/sda "mklabel gpt" 

    [root@centos-stream9-n2 ~]# parted --script /dev/sda "mkpart primary 0% 100%" 

    [root@centos-stream9-n2 ~]#  mkfs.xfs /dev/sda1 
    meta-data=/dev/sda1              isize=512    agcount=4, agsize=326656 blks
             =                       sectsz=512   attr=2, projid32bit=1
             =                       crc=1        finobt=1, sparse=1, rmapbt=0
             =                       reflink=1    bigtime=1 inobtcount=1 nrext64=0
    data     =                       bsize=4096   blocks=1306624, imaxpct=25
             =                       sunit=0      swidth=0 blks
    naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
    log      =internal log           bsize=4096   blocks=16384, version=2
             =                       sectsz=512   sunit=0 blks, lazy-count=1
    realtime =none                   extsz=4096   blocks=0, rtextents=0

    [root@centos-stream9-n2 ~]# mount /dev/sda1 /mnt 

    [root@centos-stream9-n2 ~]# df -Th /mnt
    Filesystem     Type  Size  Used Avail Use% Mounted on
    /dev/sda1      xfs   5.0G   68M  4.9G   2% /mnt
    
    
Logout the session. 

    [root@centos-stream9-n2 ~]# iscsiadm -m node --logout -T  iqn.2025-09.redhat-workshop.centos-stream9:n1.target01 -p 192.168.1.156 
    Logging out of session [sid: 1, target: iqn.2025-09.redhat-workshop.centos-stream9:n1.target01, portal: 192.168.1.156,3260]
    Logout of [sid: 1, target: iqn.2025-09.redhat-workshop.centos-stream9:n1.target01, portal: 192.168.1.156,3260] successful.

    [root@centos-stream9-n2 ~]# lsblk
    NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
    sr0          11:0    1 1024M  0 rom  
    vda         252:0    0   20G  0 disk 
    ├─vda1      252:1    0    1G  0 part /boot
    └─vda2      252:2    0   19G  0 part 
      ├─cs-root 253:0    0   17G  0 lvm  /
      └─cs-swap 253:1    0    2G  0 lvm  [SWAP]

    