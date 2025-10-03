# DHCP: Configure DHCP Server  	

Configure DHCP ( Dynamic Host Configuration Protocol ) Server to assign IP addresses to client hosts in local network.

### [1] Install and Configure DHCP. On this example, it shows only for IPv4 configuration. 
    
    [root@centos-stream9-n1 ~]#  dnf -y install dhcp-server 
    
    [root@centos-stream9-n1 ~]# vi /etc/dhcp/dhcpd.conf 
    # create new

    # specify domain name
    option domain-name     "ocp2.redhat-workshop.in";
    # specify DNS server's hostname or IP address
    option domain-name-servers    centos-stream9-n1.ocp2.redhat-workshop.in;
    # default lease time
    default-lease-time 600;
    # max lease time
    
    max-lease-time 7200;
    # this DHCP server to be declared valid
    
    authoritative;
    # specify network address and subnetmask
    
    subnet 192.168.1.0 netmask 255.255.255.0 {
        # specify the range of lease IP address
        range dynamic-bootp 192.168.1.60 192.168.1.70;
        # specify broadcast address
        option broadcast-address 192.168.1.255;
        # specify gateway
        option routers 192.168.1.1;
    }
    host centos-stream9-n2 {
      hardware ethernet 52:54:00:ba:85:5a;
      fixed-address 192.168.1.157;
      server-name "centos-stream9-n2.ocp2.redhat-workshop.in";
    }
    
    host centos-stream9-n3 {
      hardware ethernet 52:54:00:aa:f4:82;
      fixed-address 192.168.1.158;
      server-name "centos-stream9-n3.ocp2.redhat-workshop.in";
    }
       

[root@centos-stream9-n1 ~]# systemctl enable --now dhcpd 
Created symlink /etc/systemd/system/multi-user.target.wants/dhcpd.service → /usr/lib/systemd/system/dhcpd.service.

### [2] If Firewalld is running, allow DHCP service. DHCP Server uses [67/UDP].

    [root@centos-stream9-n1 ~]# firewall-cmd --add-service=dhcp
    success
    [root@centos-stream9-n1 ~]# firewall-cmd --runtime-to-permanent    
    success

### [3] It's possible to see leased IP address in the file below from DHCP Server to DHCP Clients. 

    [root@centos-stream9-n1 ~]# ll /var/lib/dhcpd 
    total 4
    -rw-r--r--. 1 dhcpd dhcpd   0 Apr 12  2023 dhcpd6.leases
    -rw-r--r--. 1 dhcpd dhcpd 279 Oct  4 04:55 dhcpd.leases
    -rw-r--r--. 1 dhcpd dhcpd   0 Apr 12  2023 dhcpd.leases~
        
    [root@centos-stream9-n1 ~]# vi /etc/sysconfig/dhcpd 
    DHCPARGS=enp1s0

    [root@centos-stream9-n1 ~]# systemctl enable --now dhcpd 

    [root@centos-stream9-n1 ~]# systemctl restart dhcpd 

    [root@centos-stream9-n1 ~]# systemctl status dhcpd 
    ● dhcpd.service - DHCPv4 Server Daemon
         Loaded: loaded (/usr/lib/systemd/system/dhcpd.service; enabled; preset: disabled)
         Active: active (running) since Sat 2025-10-04 05:02:16 IST; 3s ago
           Docs: man:dhcpd(8)
                 man:dhcpd.conf(5)
       Main PID: 13989 (dhcpd)
         Status: "Dispatching packets..."
          Tasks: 1 (limit: 10637)
         Memory: 5.0M (peak: 5.3M)
            CPU: 4ms
         CGroup: /system.slice/dhcpd.service
                 └─13989 /usr/sbin/dhcpd -f -cf /etc/dhcp/dhcpd.conf -user dhcpd -group dhcpd --no-pid
    
    Oct 04 05:02:16 centos-stream9-n1.ocp2.redhat-workshop.in dhcpd[13989]: PID file: /var/run/dhcpd.pid
    Oct 04 05:02:16 centos-stream9-n1.ocp2.redhat-workshop.in dhcpd[13989]: Source compiled to use binary-leases
    Oct 04 05:02:16 centos-stream9-n1.ocp2.redhat-workshop.in dhcpd[13989]: Wrote 0 deleted host decls to leases file.
    Oct 04 05:02:16 centos-stream9-n1.ocp2.redhat-workshop.in dhcpd[13989]: Wrote 0 new dynamic host decls to leases file.
    Oct 04 05:02:16 centos-stream9-n1.ocp2.redhat-workshop.in dhcpd[13989]: Wrote 0 leases to leases file.
    Oct 04 05:02:16 centos-stream9-n1.ocp2.redhat-workshop.in dhcpd[13989]: Listening on LPF/enp1s0/52:54:00:8e:24:c1/192.168.1.0/24
    Oct 04 05:02:16 centos-stream9-n1.ocp2.redhat-workshop.in dhcpd[13989]: Sending on   LPF/enp1s0/52:54:00:8e:24:c1/192.168.1.0/24
    Oct 04 05:02:16 centos-stream9-n1.ocp2.redhat-workshop.in dhcpd[13989]: Sending on   Socket/fallback/fallback-net
    Oct 04 05:02:16 centos-stream9-n1.ocp2.redhat-workshop.in dhcpd[13989]: Server starting service.
    Oct 04 05:02:16 centos-stream9-n1.ocp2.redhat-workshop.in systemd[1]: Started DHCPv4 Server Daemon.

    