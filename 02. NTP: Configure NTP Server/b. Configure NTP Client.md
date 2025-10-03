1] 	NTP Client configuration is mostly the same with the Server's one, however, NTP Clients do not need to receive time synchronization requests from other hosts, so it does not need to specify the line [allow ***]. 

    [root@centos-stream9-n2 ~]# dnf -y install chrony 
    Last metadata expiration check: 1:17:31 ago on Friday 03 October 2025 10:10:43 PM.
    Package chrony-4.6.1-2.el9.x86_64 is already installed.
    Dependencies resolved.
    Nothing to do.
    Complete!

    [root@centos-stream9-n2 ~]# vi /etc/chrony.conf 
    #
    pool 2.centos.pool.ntp.org iburst
    pool 192.168.1.252 iburst

    [root@centos-stream9-n2 ~]# systemctl enable --now chronyd
    
    # verify status

    [root@centos-stream9-n2 ~]# chronyc sources
    MS Name/IP address         Stratum Poll Reach LastRx Last sample               
    ===============================================================================
    ^? bastion2.ocp2.redhat-wor>     3   6     3     1  -2164us[-2164us] +/-   41ms
    
[2] To Install NTPStat, it's possible to display time synchronization status. 

    [root@centos-stream9-n2 ~]# dnf -y install ntpstat 

    [root@centos-stream9-n2 ~]# ntpstat
    synchronised to NTP server (192.168.1.252) at stratum 4
       time correct to within 41 ms
       polling server every 64 s
    