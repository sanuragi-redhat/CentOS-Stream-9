 	
Install Chrony to Configure NTP Server for Time Synchronization. 

[1] 	Install and Configure Chrony. 

    [root@centos-stream9-n1 ~]# dnf -y install chrony 
    Last metadata expiration check: 1:17:31 ago on Friday 03 October 2025 10:10:43 PM.
    Package chrony-4.6.1-2.el9.x86_64 is already installed.
    Dependencies resolved.
    Nothing to do.
    Complete!
    
    [root@centos-stream9-n1 ~]# vi /etc/chrony.conf 
    # line 3 : change servers to synchronize (replace to your own timezone NTP server)
    # need NTP server itself to sync time with other NTP server
    
    #
    pool 2.centos.pool.ntp.org iburst
    pool 192.168.1.252 iburst

    # line 27 : add network range to allow to receive time synchronization requests from NTP Clients
    # specify your local network and so on
    # if not specified, only localhost is allowed
    
    allow 192.168.1.0/24
    
[2] 	If Firewalld is running, allow NTP service. NTP uses [123/UDP]. 

    [root@centos-stream9-n1 ~]# firewall-cmd --add-service=ntp 
    [root@centos-stream9-n1 ~]# firewall-cmd --runtime-to-permanent 

[3] 	Verify it works normally. 

    [root@centos-stream9-n1 ~]# chronyc sources 
    MS Name/IP address         Stratum Poll Reach LastRx Last sample               
    ===============================================================================
    ^* bastion2.ocp2.redhat-wor>     3   6    17     4  +3024ns[  +24us] +/-   74ms
    