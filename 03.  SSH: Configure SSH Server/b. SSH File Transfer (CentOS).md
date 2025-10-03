[1] 	It's the example for using SCP (Secure Copy).

    # command ⇒ scp [Option] Source Target
    # copy the [test.txt] on localhost to remote host [centos-stream9-n2]
    [cent@redhat@centos-stream9-n1 ~]$ scp ./test.txt redhat@centos-stream9-n2:~/
    
    redhat@centos-stream9-n2's password:     # password of the user
    test.txt                                      100%   10     0.0KB/s   00:00
    
# copy the [/home/redhat/test.txt] on remote host [centos-stream9-n2] to the localhost

    [cent@redhat@centos-stream9-n2 ~]$ scp redhat@centos-stream9-n1:/home/redhat/test.txt ./test.txt
    
    redhat@centos-stream9-n1's password:
    test.txt                                      100%   10     0.0KB/s   00:00

[2] 	It's the examples to use SFTP (SSH File Transfer Protocol).
SFTP server feature is enabled by default, but if not, enable it to add the line [Subsystem sftp /usr/libexec/openssh/sftp-server] in [/etc/ssh/sshd_config].
    
    # sftp [Option] [user@host]
    [redhat@redhat@centos-stream9-n1 ~]$ sftp redhat@centos-stream9-n1
    
    redhat@centos-stream9-n1's password:    # password of the user
    Connected to centos-stream9-n2.
    sftp>
    
    # show current directory on remote host
    sftp> pwd
    Remote working directory: /home/redhat
    
    # show current directory on localhost
    sftp> !pwd
    /home/redhat 
    
    # show files in current directory on remote host
    sftp> ls -l
    drwxrwxr-x    2 redhat   redhat            7 Jan 04 21:33 public_html
    -rw-rw-r--    1 redhat   redhat           10 Jan 04 22:53 test.txt
    
    # show files in current directory on localhost
    sftp> !ls -l
    total 4
    -rw-rw-r--    1 redhat     redhat       10 Jan 04 21:53 test.txt
    
    # change directory
    sftp> cd public_html
    sftp> pwd
    Remote working directory: /home/redhat/public_html
    
    # upload a file to remote host
    sftp> put test.txt redhat.txt
    Uploading test.txt to /home/redhat/redhat.txt
    test.txt                           100%   10     0.0KB/s   00:00
    sftp> ls -l
    drwxrwxr-x    2 redhat   redhat            6 Jan 04 21:33 public_html
    -rw-rw-r--    1 redhat   redhat           10 Jan 04 21:39 redhat.txt
    -rw-rw-r--    1 redhat   redhat           10 Jan 04 22:53 test.txt
    
    # upload some files to remote host
    sftp> put *.txt
    Uploading test.txt to /home/redhat/test.txt
    test.txt                                           100%   10     0.0KB/s   00:00
    Uploading test2.txt to /home/redhat/test2.txt
    test2.txt                                          100%    0     0.0KB/s   00:00
    sftp> ls -l
    drwxrwxr-x    2 redhat   redhat            6 Jan 04 21:33 public_html
    -rw-rw-r--    1 redhat   redhat           10 Jan 04 21:39 redhat.txt
    -rw-rw-r--    1 redhat   redhat           10 Jan 04 21:45 test.txt
    -rw-rw-r--    1 redhat   redhat           10 Jan 04 21:46 test2.txt
    
    # download a file from remote host
    sftp> get test.txt
    Fetching /home/redhat/test.txt to test.txt
    /home/redhat/test.txt                   100%   10     0.0KB/s   00:00
    
    # download some files from remote host
    sftp> get *.txt
    Fetching /home/redhat/cent.txt to cent.txt
    /home/redhat/cent.txt                                       100%   10     0.0KB/s   00:00
    Fetching /home/redhat/test.txt to test.txt
    /home/redhat/test.txt                                         100%   10     0.0KB/s   00:00
    Fetching /home/redhat/test2.txt to test2.txt
    /home/redhat/test2.txt                                        100%   10     0.0KB/s   00:00
    
    # create a directory on remote host
    sftp> mkdir testdir
    sftp> ls -l
    drwxrwxr-x    2 redhat   redhat            6 Jan 04 21:33 public_html
    -rw-rw-r--    1 redhat   redhat           10 Jan 04 21:39 redhat.txt
    -rw-rw-r--    1 redhat   redhat           10 Jan 04 21:45 test.txt
    -rw-rw-r--    1 redhat   redhat           10 Jan 04 21:46 test2.txt
    drwxrwxr-x    2 redhat   redhat            6 Jan 04 21:53 testdir
    
    # delete a directory on remote host
    sftp> rmdir testdir
    rmdir ok, `testdir' removed
    sftp> ls -l
    drwxrwxr-x    2 redhat   redhat            6 Jan 04 21:33 public_html
    -rw-rw-r--    1 redhat   redhat           10 Jan 04 21:39 redhat.txt
    -rw-rw-r--    1 redhat   redhat           10 Jan 04 21:45 test.txt
    -rw-rw-r--    1 redhat   redhat           10 Jan 04 21:46 test2.txt
    
    # delete a file on remote host
    sftp> rm test2.txt
    Removing /home/redhat/test2.txt
    sftp> ls -l
    drwxrwxr-x    2 redhat   redhat            6 Jan 04 21:33 public_html
    -rw-rw-r--    1 redhat   redhat           10 Jan 04 21:39 redhat.txt
    -rw-rw-r--    1 redhat   redhat           10 Jan 04 21:45 test.txt
    
    # execute commands with ![command]
    sftp> !cat /etc/passwd
    root:x:0:0:root:/root:/bin/bash
    bin:x:1:1:bin:/bin:/sbin/nologin
    .....
    .....
    redhat:x:1001:1001::/home/redhat:/bin/bash
    
    # exit
    sftp> quit
    221 Goodbye.
