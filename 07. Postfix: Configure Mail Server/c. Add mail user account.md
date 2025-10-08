Add Mail User Accounts to use Mail Service. 

    # install mail client program    
    [root@centos-stream9-n1 ~]#  dnf -y install s-nail 
    
    # set environment variables to use Maildir
    [root@centos-stream9-n1 ~]# echo 'export MAIL=$HOME/Maildir' >> /etc/profile.d/mail.sh 
    
To use OS user accounts, that's only adding OS user like follows.
    
    # add a user [cent]
    [root@centos-stream9-n1 ~]# useradd cent
    [root@centos-stream9-n1 ~]# passwd cent
    Changing password for user cent.
    New password: password
    BAD PASSWORD: The password is shorter than 8 characters
    Retype new password: password
    passwd: all authentication tokens updated successfully.
    

Login as a user added in [1] and try to send an email. 

    # send to myself [mail (username)@(hostname)]
    [cent@centos-stream9-n1 ~]$ mail cent@localhost
    
    # input subject
    Subject: Test Mail#1
    # input messages
    This is the first mail.
    
    # to finish messages, push Ctrl + D key
    -------
    (Preliminary) Envelope contains:
    To: cent@localhost
    Subject: Test Mail#1
    Send this message [yes/no, empty: recompose]? yes
    
    # see received emails
    
    [cent@centos-stream9-n1 ~]$ mail 
    s-nail version v14.9.22.  Type `?' for help
    /home/cent/Maildir: 1 message 1 new
    ▸N  1 root                  2025-10-04 07:54   14/471   "Test Mail#1                                                                                      "
    & 
    [-- Message  1 -- 14 lines, 471 bytes --]:
    Date: Sat, 04 Oct 2025 07:54:38 +0530
    To: cent@localhost
    Subject: Test Mail#1
    Message-Id: <20251004022438.8C9BA20613B0@mail.ocp2.redhat-workshop.in>
    From: root <root@ocp2.redhat-workshop.in>
    
    This is the first mail.
    
    & q
    Held 1 message in /home/cent/Maildir
    
    