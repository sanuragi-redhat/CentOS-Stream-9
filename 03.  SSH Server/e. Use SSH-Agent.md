### OpenSSH: Use SSH-agent

  	
Use SSH-Agent to automate inputting Passphrase on SSH Key Pair authentication.
SSH-Agent is useful for users who set SSH Key Pair with Passphrase.

[1] 	This is some example to use SSH-Agent. 
    
    # run SSH-Agent
    [redhat@centos-stream9-n1 ~]$ eval $(ssh-agent) 
    Agent pid 2052
    
    
    [redhat@centos-stream9-n1 ~]$ ssh-add 
    Identity added: /home/redhat/.ssh/id_ed25519 (redhat@centos-stream9-n1.ocp2.redhat-workshop.in)
    
    # verify
    [redhat@centos-stream9-n1 ~]$ ssh-add -l 
    256 SHA256:6oxHR3YzPvDbrYHbbRoeFS3HWRqsjhmuvRMaFhnDWTE redhat@centos-stream9-n1.ocp2.redhat-workshop.in (ED25519)
    
    # try to connect without input
    [redhat@centos-stream9-n1 ~]$ ssh redhat@centos-stream9-n2 hostname 
    centos-stream9-n2.ocp2.redhat-workshop.in

    # stop SSH-agent
    # if not execute it, SSH-Agent process remains even if you logout System, be careful
    [redhat@centos-stream9-n1 ~]$ eval $(ssh-agent -k)
    Agent pid 2052 killed
    [redhat@centos-stream9-n1 ~]$ 
    