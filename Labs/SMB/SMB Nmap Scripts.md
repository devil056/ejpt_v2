### SMB2

**Objective:**
1. Identify SMB Protocol Dialects
2. Find SMB security level information
3. Enumerate active sessions, shares, Windows users, domains, services, etc.
The following username and password may be used to access the service:
| Username | Password | | administrator | smbserver_771 |

`ris:PriceTag3` [[Servers and Services#SMB|SMB]] [[Hack2.0/NMAP|NMAP]]
`ris:Tools` [[Hack2.0/NMAP|NMAP]]

_**Tagret IP : 10.5.18.239**_

[[ip]]
```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
1585: eth0@if1586: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:03 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.3/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
1587: eth1@if1588: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:0a:1a:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.10.26.2/24 brd 10.10.26.255 scope global eth1
       valid_lft forever preferred_lft forever
```


[[ping]]

```
root@attackdefense:~# ping -c 5 10.5.18.239
PING 10.5.18.239 (10.5.18.239) 56(84) bytes of data.
64 bytes from 10.5.18.239: icmp_seq=1 ttl=125 time=3.61 ms
64 bytes from 10.5.18.239: icmp_seq=2 ttl=125 time=2.86 ms
64 bytes from 10.5.18.239: icmp_seq=3 ttl=125 time=2.82 ms
64 bytes from 10.5.18.239: icmp_seq=4 ttl=125 time=2.88 ms
64 bytes from 10.5.18.239: icmp_seq=5 ttl=125 time=2.90 ms

--- 10.5.18.239 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4008ms
rtt min/avg/max/mdev = 2.818/3.013/3.611/0.300 ms
```

[[Hack2.0/NMAP#General Scan|NMAP General scan]]

```
root@attackdefense:~# nmap 10.5.18.239
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-07 09:53 IST
Nmap scan report for 10.5.18.239
Host is up (0.0025s latency).
Not shown: 992 closed ports
PORT      STATE SERVICE
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
3389/tcp  open  ms-wbt-server
49152/tcp open  unknown
49153/tcp open  unknown
49154/tcp open  unknown
49155/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 1.54 seconds
```

[[Hack2.0/NMAP#Service version|NMAP Service version]]

```
root@attackdefense:~# nmap -p 135,139,445,3389,49152,49153,49154,49155 -sV 10.5.18.239
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-07 09:55 IST
Nmap scan report for 10.5.18.239
Host is up (0.0026s latency).

PORT      STATE SERVICE            VERSION
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
3389/tcp  open  ssl/ms-wbt-server?
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49155/tcp open  msrpc              Microsoft Windows RPC
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 65.46 seconds
```

The Nmap scripts are located in the /usr/share/nmap/scripts out of them search for the Smb based scripts for enumeration and run them. Below are the scripts that are available at the moment of this lab.

```
root@attackdefense:/usr/share/nmap/scripts# ls | grep "smb"
smb-brute.nse
smb-double-pulsar-backdoor.nse
smb-enum-domains.nse
smb-enum-groups.nse
smb-enum-processes.nse
smb-enum-services.nse
smb-enum-sessions.nse
smb-enum-shares.nse
smb-enum-users.nse
smb-flood.nse
smb-ls.nse
smb-mbenum.nse
smb-os-discovery.nse
smb-print-text.nse
smb-protocols.nse
smb-psexec.nse
smb-security-mode.nse
smb-server-stats.nse
smb-system-info.nse
smb-vuln-conficker.nse
smb-vuln-cve-2017-7494.nse
smb-vuln-cve2009-3103.nse
smb-vuln-ms06-025.nse
smb-vuln-ms07-029.nse
smb-vuln-ms08-067.nse
smb-vuln-ms10-054.nse
smb-vuln-ms10-061.nse
smb-vuln-ms17-010.nse
smb-vuln-regsvc-dos.nse
smb-vuln-webexec.nse
smb-webexec-exploit.nse
smb2-capabilities.nse
smb2-security-mode.nse
smb2-time.nse
smb2-vuln-uptime.nse
```

```
┌──(murari㉿kali)-[~]
└─$ grep "smb" /usr/share/nmap/scripts/script.db       
Entry { filename = "smb-brute.nse", categories = { "brute", "intrusive", } }
Entry { filename = "smb-double-pulsar-backdoor.nse", categories = { "malware", "safe", "vuln", } }
Entry { filename = "smb-enum-domains.nse", categories = { "discovery", "intrusive", } }
Entry { filename = "smb-enum-groups.nse", categories = { "discovery", "intrusive", } }
Entry { filename = "smb-enum-processes.nse", categories = { "discovery", "intrusive", } }
Entry { filename = "smb-enum-services.nse", categories = { "discovery", "intrusive", "safe", } }
Entry { filename = "smb-enum-sessions.nse", categories = { "discovery", "intrusive", } }
Entry { filename = "smb-enum-shares.nse", categories = { "discovery", "intrusive", } }
Entry { filename = "smb-enum-users.nse", categories = { "auth", "intrusive", } }
Entry { filename = "smb-flood.nse", categories = { "dos", "intrusive", } }
Entry { filename = "smb-ls.nse", categories = { "discovery", "safe", } }
Entry { filename = "smb-mbenum.nse", categories = { "discovery", "safe", } }
Entry { filename = "smb-os-discovery.nse", categories = { "default", "discovery", "safe", } }
Entry { filename = "smb-print-text.nse", categories = { "intrusive", } }
Entry { filename = "smb-protocols.nse", categories = { "discovery", "safe", } }
Entry { filename = "smb-psexec.nse", categories = { "intrusive", } }
Entry { filename = "smb-security-mode.nse", categories = { "default", "discovery", "safe", } }
Entry { filename = "smb-server-stats.nse", categories = { "discovery", "intrusive", } }
Entry { filename = "smb-system-info.nse", categories = { "discovery", "intrusive", } }
Entry { filename = "smb-vuln-conficker.nse", categories = { "dos", "exploit", "intrusive", "vuln", } }
Entry { filename = "smb-vuln-cve-2017-7494.nse", categories = { "intrusive", "vuln", } }
Entry { filename = "smb-vuln-cve2009-3103.nse", categories = { "dos", "exploit", "intrusive", "vuln", } }
Entry { filename = "smb-vuln-ms06-025.nse", categories = { "dos", "exploit", "intrusive", "vuln", } }
Entry { filename = "smb-vuln-ms07-029.nse", categories = { "dos", "exploit", "intrusive", "vuln", } }
Entry { filename = "smb-vuln-ms08-067.nse", categories = { "dos", "exploit", "intrusive", "vuln", } }
Entry { filename = "smb-vuln-ms10-054.nse", categories = { "dos", "intrusive", "vuln", } }
Entry { filename = "smb-vuln-ms10-061.nse", categories = { "intrusive", "vuln", } }
Entry { filename = "smb-vuln-ms17-010.nse", categories = { "safe", "vuln", } }
Entry { filename = "smb-vuln-regsvc-dos.nse", categories = { "dos", "exploit", "intrusive", "vuln", } }
Entry { filename = "smb-vuln-webexec.nse", categories = { "intrusive", "vuln", } }
Entry { filename = "smb-webexec-exploit.nse", categories = { "exploit", "intrusive", } }
Entry { filename = "smb2-capabilities.nse", categories = { "discovery", "safe", } }
Entry { filename = "smb2-security-mode.nse", categories = { "default", "discovery", "safe", } }
Entry { filename = "smb2-time.nse", categories = { "default", "discovery", "safe", } }
Entry { filename = "smb2-vuln-uptime.nse", categories = { "safe", "vuln", } }
```

[[Hack2.0/NMAP#Specific script|NMAP Specific script]]

```
root@attackdefense:~# nmap --script smb-protocols -p 445 10.5.18.239
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-07 10:01 IST
Nmap scan report for 10.5.18.239
Host is up (0.0029s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-protocols: 
|   dialects: 
|     NT LM 0.12 (SMBv1) [dangerous, but default]
|     2.02
|     2.10
|     3.00
|_    3.02

Nmap done: 1 IP address (1 host up) scanned in 6.43 seconds
```

```
root@attackdefense:~# nmap --script smb-security-mode -p 445 10.5.18.239
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-07 10:04 IST
Nmap scan report for 10.5.18.239
Host is up (0.0030s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)

Nmap done: 1 IP address (1 host up) scanned in 1.39 seconds
```

```
root@attackdefense:~# nmap --script smb-enum-sessions -p 445 10.5.18.239
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-07 10:30 IST
Nmap scan report for 10.5.18.239
Host is up (0.0024s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-enum-sessions: 
|   Users logged in
|_    WIN-OMCNBKR66MN\bob since <unknown>

Nmap done: 1 IP address (1 host up) scanned in 3.78 seconds
```

We will try to find the sessions using the username and the password provided as well.

**Note: Due to crash of the server had to restart the lab so you will find a different IP address now going forward.**

```
root@attackdefense:~# nmap -p 445 --script smb-enum-sessions.nse --script-args smbusername=administrator,smbpassword=smbserver_771 10.5.30.137
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-07 11:52 IST
Nmap scan report for 10.5.30.137
Host is up (0.0017s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-enum-sessions: 
|   Users logged in
|     WIN-OMCNBKR66MN\bob since 2023-07-07T06:16:59
|   Active SMB sessions
|_    ADMINISTRATOR is connected from \\10.10.12.4 for [just logged in, it's probably you], idle for [not idle]

Nmap done: 1 IP address (1 host up) scanned in 3.51 seconds
```

```
root@attackdefense:~# nmap -p 445 --script smb-enum-shares.nse 10.5.30.137
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-07 11:53 IST
Nmap scan report for 10.5.30.137
Host is up (0.0018s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-enum-shares: 
|   account_used: guest
|   \\10.5.30.137\ADMIN$: 
|     Type: STYPE_DISKTREE_HIDDEN
|     Comment: Remote Admin
|     Anonymous access: <none>
|     Current user access: <none>
|   \\10.5.30.137\C: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Anonymous access: <none>
|     Current user access: READ
|   \\10.5.30.137\C$: 
|     Type: STYPE_DISKTREE_HIDDEN
|     Comment: Default share
|     Anonymous access: <none>
|     Current user access: <none>
|   \\10.5.30.137\D$: 
|     Type: STYPE_DISKTREE_HIDDEN
|     Comment: Default share
|     Anonymous access: <none>
|     Current user access: <none>
|   \\10.5.30.137\Documents: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Anonymous access: <none>
|     Current user access: READ
|   \\10.5.30.137\Downloads: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Anonymous access: <none>
|     Current user access: READ
|   \\10.5.30.137\IPC$: 
|     Type: STYPE_IPC_HIDDEN
|     Comment: Remote IPC
|     Anonymous access: <none>
|     Current user access: READ/WRITE
|   \\10.5.30.137\print$: 
|     Type: STYPE_DISKTREE
|     Comment: Printer Drivers
|     Anonymous access: <none>
|_    Current user access: READ

Nmap done: 1 IP address (1 host up) scanned in 43.97 seconds
```

```
root@attackdefense:~# nmap --script smb-enum-shares --script-args smbusername=administrator,smbpassword=smbserver_771 -p445 10.5.30.137
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-07 11:59 IST
Nmap scan report for 10.5.30.137
Host is up (0.0019s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-enum-shares: 
|   account_used: administrator
|   \\10.5.30.137\ADMIN$: 
|     Type: STYPE_DISKTREE_HIDDEN
|     Comment: Remote Admin
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\Windows
|     Anonymous access: <none>
|     Current user access: READ/WRITE
|   \\10.5.30.137\C: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\
|     Anonymous access: <none>
|     Current user access: READ
|   \\10.5.30.137\C$: 
|     Type: STYPE_DISKTREE_HIDDEN
|     Comment: Default share
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\
|     Anonymous access: <none>
|     Current user access: READ/WRITE
|   \\10.5.30.137\D$: 
|     Type: STYPE_DISKTREE_HIDDEN
|     Comment: Default share
|     Users: 0
|     Max Users: <unlimited>
|     Path: D:\
|     Anonymous access: <none>
|     Current user access: READ/WRITE
|   \\10.5.30.137\Documents: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\Users\Administrator\Documents
|     Anonymous access: <none>
|     Current user access: READ
|   \\10.5.30.137\Downloads: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\Users\Administrator\Downloads
|     Anonymous access: <none>
|     Current user access: READ
|   \\10.5.30.137\IPC$: 
|     Type: STYPE_IPC_HIDDEN
|     Comment: Remote IPC
|     Users: 1
|     Max Users: <unlimited>
|     Path: 
|     Anonymous access: <none>
|     Current user access: READ/WRITE
|   \\10.5.30.137\print$: 
|     Type: STYPE_DISKTREE
|     Comment: Printer Drivers
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\Windows\system32\spool\drivers
|     Anonymous access: <none>
|_    Current user access: READ/WRITE

Nmap done: 1 IP address (1 host up) scanned in 48.07 seconds
```

We can see that the anonymous access has read/write access so we will try to check the users as well just to be safe.

```
root@attackdefense:~# nmap --script smb-enum-users --script-args smbusername=administrator,smbpassword=smbserver_771 -p445 10.5.30.137
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-07 12:02 IST
Nmap scan report for 10.5.30.137
Host is up (0.0018s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-enum-users: 
|   WIN-OMCNBKR66MN\Administrator (RID: 500)
|     Description: Built-in account for administering the computer/domain
|     Flags:       Normal user account, Password does not expire
|   WIN-OMCNBKR66MN\bob (RID: 1010)
|     Flags:       Normal user account, Password does not expire
|   WIN-OMCNBKR66MN\Guest (RID: 501)
|     Description: Built-in account for guest access to the computer/domain
|_    Flags:       Normal user account, Password not required, Password does not expire

Nmap done: 1 IP address (1 host up) scanned in 4.47 seconds
```

```
root@attackdefense:~# nmap --script smb-server-stats --script-args smbusername=administrator,smbpassword=smbserver_771 -p445 10.5.30.137
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-07 12:04 IST
Nmap scan report for 10.5.30.137
Host is up (0.0018s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-server-stats: 
|   Server statistics collected since 2023-07-07T06:16:50 (17m16s):
|     89335 bytes (86.23 b/s) sent, 75270 bytes (72.65 b/s) received
|_    34 failed logins, 7 permission errors, 0 system errors, 0 print jobs, 34 files opened

Nmap done: 1 IP address (1 host up) scanned in 1.41 seconds
```

```
root@attackdefense:~# nmap --script smb-enum-domains --script-args smbusername=administrator,smbpassword=smbserver_771 -p445 10.5.30.137
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-07 12:05 IST
Nmap scan report for 10.5.30.137
Host is up (0.0018s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-enum-domains: 
|   WIN-OMCNBKR66MN
|     Groups: WinRMRemoteWMIUsers__
|     Users: Administrator, bob, Guest
|     Creation time: 2013-08-22T14:47:57
|     Passwords: min length: n/a; min age: n/a days; max age: 42 days; history: n/a passwords
|     Properties: Complexity requirements exist
|     Account lockout disabled
|   Builtin
|     Groups: Access Control Assistance Operators, Administrators, Backup Operators, Certificate Service DCOM Access, Cryptographic Operators, Distributed COM Users, Event Log Readers, Guests, Hyper-V Administrators, IIS_IUSRS, Network Configuration Operators, Performance Log Users, Performance Monitor Users, Power Users, Print Operators, RDS Endpoint Servers, RDS Management Servers, RDS Remote Access Servers, Remote Desktop Users, Remote Management Users, Replicator, Users
|     Users: n/a
|     Creation time: 2013-08-22T14:47:57
|     Passwords: min length: n/a; min age: n/a days; max age: 42 days; history: n/a passwords
|_    Account lockout disabled

Nmap done: 1 IP address (1 host up) scanned in 3.54 seconds

```

```
root@attackdefense:~# nmap --script smb-enum-groups --script-args smbusername=administrator,smbpassword=smbserver_771 -p445 10.5.30.137
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-07 12:06 IST
Nmap scan report for 10.5.30.137
Host is up (0.0019s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-enum-groups: 
|   Builtin\Administrators (RID: 544): Administrator, bob
|   Builtin\Users (RID: 545): bob
|   Builtin\Guests (RID: 546): Guest
|   Builtin\Power Users (RID: 547): <empty>
|   Builtin\Print Operators (RID: 550): <empty>
|   Builtin\Backup Operators (RID: 551): <empty>
|   Builtin\Replicator (RID: 552): <empty>
|   Builtin\Remote Desktop Users (RID: 555): bob
|   Builtin\Network Configuration Operators (RID: 556): <empty>
|   Builtin\Performance Monitor Users (RID: 558): <empty>
|   Builtin\Performance Log Users (RID: 559): <empty>
|   Builtin\Distributed COM Users (RID: 562): <empty>
|   Builtin\IIS_IUSRS (RID: 568): <empty>
|   Builtin\Cryptographic Operators (RID: 569): <empty>
|   Builtin\Event Log Readers (RID: 573): <empty>
|   Builtin\Certificate Service DCOM Access (RID: 574): <empty>
|   Builtin\RDS Remote Access Servers (RID: 575): <empty>
|   Builtin\RDS Endpoint Servers (RID: 576): <empty>
|   Builtin\RDS Management Servers (RID: 577): <empty>
|   Builtin\Hyper-V Administrators (RID: 578): <empty>
|   Builtin\Access Control Assistance Operators (RID: 579): <empty>
|   Builtin\Remote Management Users (RID: 580): <empty>
|_  WIN-OMCNBKR66MN\WinRMRemoteWMIUsers__ (RID: 1000): <empty>

Nmap done: 1 IP address (1 host up) scanned in 2.70 seconds
```

```
root@attackdefense:~# nmap --script smb-enum-services --script-args smbusername=administrator,smbpassword=smbserver_771 -p445 10.5.30.137
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-07 12:06 IST
Nmap scan report for 10.5.30.137
Host is up (0.0019s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds
| smb-enum-services: 
|   AmazonSSMAgent: 
|     display_name: Amazon SSM Agent
|     state: 
|       SERVICE_PAUSE_PENDING
|       SERVICE_PAUSED
|       SERVICE_CONTINUE_PENDING
|       SERVICE_RUNNING
|     type: 
|       SERVICE_TYPE_WIN32
|       SERVICE_TYPE_WIN32_OWN_PROCESS
|     controls_accepted: 
|       SERVICE_CONTROL_INTERROGATE
|       SERVICE_CONTROL_PARAMCHANGE
|       SERVICE_CONTROL_CONTINUE
|       SERVICE_CONTROL_NETBINDENABLE
|       SERVICE_CONTROL_NETBINDADD
|       SERVICE_CONTROL_STOP
|   AWSLiteAgent: 
|     display_name: AWS Lite Guest Agent
|     state: 
|       SERVICE_PAUSE_PENDING
|       SERVICE_PAUSED
|       SERVICE_CONTINUE_PENDING
|       SERVICE_RUNNING
|     type: 
|       SERVICE_TYPE_WIN32
|       SERVICE_TYPE_WIN32_OWN_PROCESS
|     controls_accepted: 
|       SERVICE_CONTROL_INTERROGATE
|       SERVICE_CONTROL_PARAMCHANGE
|       SERVICE_CONTROL_CONTINUE
|       SERVICE_CONTROL_NETBINDENABLE
|       SERVICE_CONTROL_NETBINDADD
|       SERVICE_CONTROL_STOP
|   DiagTrack: 
|     display_name: Diagnostics Tracking Service
|     state: 
|       SERVICE_PAUSE_PENDING
|       SERVICE_PAUSED
|       SERVICE_CONTINUE_PENDING
|       SERVICE_RUNNING
|     type: 
|       SERVICE_TYPE_WIN32
|       SERVICE_TYPE_WIN32_OWN_PROCESS
|     controls_accepted: 
|       SERVICE_CONTROL_INTERROGATE
|       SERVICE_CONTROL_PARAMCHANGE
|       SERVICE_CONTROL_CONTINUE
|       SERVICE_CONTROL_NETBINDENABLE
|       SERVICE_CONTROL_NETBINDADD
|       SERVICE_CONTROL_STOP
|   Ec2Config: 
|     display_name: Ec2Config
|     state: 
|       SERVICE_PAUSE_PENDING
|       SERVICE_PAUSED
|       SERVICE_CONTINUE_PENDING
|       SERVICE_RUNNING
|     type: 
|       SERVICE_TYPE_WIN32
|       SERVICE_TYPE_WIN32_OWN_PROCESS
|     controls_accepted: 
|       SERVICE_CONTROL_INTERROGATE
|       SERVICE_CONTROL_PARAMCHANGE
|       SERVICE_CONTROL_CONTINUE
|       SERVICE_CONTROL_NETBINDENABLE
|       SERVICE_CONTROL_NETBINDADD
|       SERVICE_CONTROL_STOP
|   MSDTC: 
|     display_name: Distributed Transaction Coordinator
|     state: 
|       SERVICE_PAUSE_PENDING
|       SERVICE_PAUSED
|       SERVICE_CONTINUE_PENDING
|       SERVICE_RUNNING
|     type: 
|       SERVICE_TYPE_WIN32
|       SERVICE_TYPE_WIN32_OWN_PROCESS
|     controls_accepted: 
|       SERVICE_CONTROL_INTERROGATE
|       SERVICE_CONTROL_PARAMCHANGE
|       SERVICE_CONTROL_CONTINUE
|       SERVICE_CONTROL_NETBINDENABLE
|       SERVICE_CONTROL_NETBINDADD
|       SERVICE_CONTROL_STOP
|   Spooler: 
|     display_name: Print Spooler
|     state: 
|       SERVICE_PAUSE_PENDING
|       SERVICE_PAUSED
|       SERVICE_CONTINUE_PENDING
|       SERVICE_RUNNING
|     type: 
|       SERVICE_TYPE_WIN32
|       SERVICE_TYPE_WIN32_OWN_PROCESS
|     controls_accepted: 
|       SERVICE_CONTROL_CONTINUE
|       SERVICE_CONTROL_NETBINDENABLE
|       SERVICE_CONTROL_NETBINDADD
|_      SERVICE_CONTROL_STOP

Nmap done: 1 IP address (1 host up) scanned in 1.45 seconds
```

I will run two scripts combined now so that we can see what files does the host hold using the smb-enum-shares and smb-ls

```
root@attackdefense:~# nmap --script smb-enum-shares,smb-ls --script-args smbusername=administrator,smbpassword=smbserver_771 -p445 10.5.30.137
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-07 12:08 IST
Nmap scan report for 10.5.30.137
Host is up (0.0018s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-enum-shares: 
|   account_used: administrator
|   \\10.5.30.137\ADMIN$: 
|     Type: STYPE_DISKTREE_HIDDEN
|     Comment: Remote Admin
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\Windows
|     Anonymous access: <none>
|     Current user access: READ/WRITE
|   \\10.5.30.137\C: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\
|     Anonymous access: <none>
|     Current user access: READ
|   \\10.5.30.137\C$: 
|     Type: STYPE_DISKTREE_HIDDEN
|     Comment: Default share
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\
|     Anonymous access: <none>
|     Current user access: READ/WRITE
|   \\10.5.30.137\D$: 
|     Type: STYPE_DISKTREE_HIDDEN
|     Comment: Default share
|     Users: 0
|     Max Users: <unlimited>
|     Path: D:\
|     Anonymous access: <none>
|     Current user access: READ/WRITE
|   \\10.5.30.137\Documents: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\Users\Administrator\Documents
|     Anonymous access: <none>
|     Current user access: READ
|   \\10.5.30.137\Downloads: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\Users\Administrator\Downloads
|     Anonymous access: <none>
|     Current user access: READ
|   \\10.5.30.137\IPC$: 
|     Type: STYPE_IPC_HIDDEN
|     Comment: Remote IPC
|     Users: 1
|     Max Users: <unlimited>
|     Path: 
|     Anonymous access: <none>
|     Current user access: READ/WRITE
|   \\10.5.30.137\print$: 
|     Type: STYPE_DISKTREE
|     Comment: Printer Drivers
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\Windows\system32\spool\drivers
|     Anonymous access: <none>
|_    Current user access: READ/WRITE
| smb-ls: Volume \\10.5.30.137\ADMIN$
|   maxfiles limit reached (10)
| SIZE   TIME                 FILENAME
| <DIR>  2013-08-22T13:36:16  .
| <DIR>  2013-08-22T13:36:16  ..
| <DIR>  2013-08-22T15:39:31  ADFS
| <DIR>  2013-08-22T15:39:31  ADFS\ar
| <DIR>  2013-08-22T15:39:31  ADFS\bg
| <DIR>  2013-08-22T15:39:31  ADFS\cs
| <DIR>  2013-08-22T15:39:31  ADFS\da
| <DIR>  2013-08-22T15:39:31  ADFS\de
| <DIR>  2013-08-22T15:39:31  ADFS\el
| <DIR>  2013-08-22T15:39:31  ADFS\en
| 
| 
| Volume \\10.5.30.137\C
|   maxfiles limit reached (10)
| SIZE   TIME                 FILENAME
| <DIR>  2013-08-22T15:39:30  PerfLogs
| <DIR>  2013-08-22T13:36:16  Program Files
| <DIR>  2014-05-17T10:36:57  Program Files\Amazon
| <DIR>  2013-08-22T13:36:16  Program Files\Common Files
| <DIR>  2014-10-15T05:58:49  Program Files\DIFX
| <DIR>  2013-08-22T15:39:31  Program Files\Internet Explorer
| <DIR>  2014-07-10T18:40:15  Program Files\Update Services
| <DIR>  2020-08-12T04:13:47  Program Files\Windows Mail
| <DIR>  2013-08-22T15:39:31  Program Files\Windows NT
| <DIR>  2013-08-22T15:39:31  Program Files\WindowsPowerShell
| 
| 
| Volume \\10.5.30.137\C$
|   maxfiles limit reached (10)
| SIZE   TIME                 FILENAME
| <DIR>  2013-08-22T15:39:30  PerfLogs
| <DIR>  2013-08-22T13:36:16  Program Files
| <DIR>  2014-05-17T10:36:57  Program Files\Amazon
| <DIR>  2013-08-22T13:36:16  Program Files\Common Files
| <DIR>  2014-10-15T05:58:49  Program Files\DIFX
| <DIR>  2013-08-22T15:39:31  Program Files\Internet Explorer
| <DIR>  2014-07-10T18:40:15  Program Files\Update Services
| <DIR>  2020-08-12T04:13:47  Program Files\Windows Mail
| <DIR>  2013-08-22T15:39:31  Program Files\Windows NT
| <DIR>  2013-08-22T15:39:31  Program Files\WindowsPowerShell
| 
| 
| Volume \\10.5.30.137\Documents
| SIZE   TIME                 FILENAME
| <DIR>  2020-09-10T09:50:27  .
| <DIR>  2020-09-10T09:50:27  ..
| 
| 
| Volume \\10.5.30.137\Downloads
| SIZE   TIME                 FILENAME
| <DIR>  2020-09-10T09:50:27  .
| <DIR>  2020-09-10T09:50:27  ..
| 
| 
| Volume \\10.5.30.137\print$
|   maxfiles limit reached (10)
| SIZE    TIME                 FILENAME
| <DIR>   2013-08-22T15:39:31  .
| <DIR>   2013-08-22T15:39:31  ..
| <DIR>   2013-08-22T15:39:31  color
| 1058    2013-08-22T06:54:44  color\D50.camp
| 1079    2013-08-22T06:54:44  color\D65.camp
| 797     2013-08-22T06:54:44  color\Graphics.gmmp
| 838     2013-08-22T06:54:44  color\MediaSim.gmmp
| 786     2013-08-22T06:54:44  color\Photo.gmmp
| 822     2013-08-22T06:54:44  color\Proofing.gmmp
| 218103  2013-08-22T06:54:44  color\RSWOP.icm
|_

Nmap done: 1 IP address (1 host up) scanned in 55.17 seconds
```

