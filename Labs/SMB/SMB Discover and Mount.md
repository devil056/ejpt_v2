### SMB 1

Objective: Discover SMB share and mount it

The following username and password may be used to access the service:

| Username | Password | | administrator | smbserver_771 |

`ris:PriceTag3` [[Servers and Services#SMB| SMB (Server Message Block)]] [[Hack2.0/NMAP|NMAP]]

`ris:Tools` [[Hack2.0/NMAP|NMAP]]

The system provided running on windows so we will use the commands related to that of windows.

```
ipconfig
```

```
PS C:\Users\Administrator> ipconfig

Windows IP Configuration


Ethernet adapter Ethernet 2:

   Connection-specific DNS Suffix  . : ap-south-1.compute.internal
   Link-local IPv6 Address . . . . . : fe80::dc14:e2b5:9f2a:800d%12
   IPv4 Address. . . . . . . . . . . : 10.5.18.1
   Subnet Mask . . . . . . . . . . . : 255.255.240.0
   Default Gateway . . . . . . . . . : 10.5.16.1

Tunnel adapter isatap.ap-south-1.compute.internal:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . : ap-south-1.compute.internal
```

By running the ipconfig command we can see that the network subnet is 255.255.240.0 meaning it is of CIDR notation 20 let us go ahead and run the network scan across the network.

[[Hack2.0/NMAP#General Scan|NMAP Scan the entire network]]

```
PS C:\Users\Administrator> nmap -T4 10.5.18.1/20 --open
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-07 03:51 Coordinated Universal Time
Nmap scan report for ip-10-5-16-44.ap-south-1.compute.internal (10.5.16.44)
Host is up (0.00s latency).
Not shown: 999 filtered ports
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT    STATE SERVICE
443/tcp open  https
MAC Address: 02:73:CE:2A:1B:90 (Unknown)

Nmap scan report for ip-10-5-28-45.ap-south-1.compute.internal (10.5.28.45)
Host is up (0.000085s latency).
Not shown: 999 filtered ports
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT    STATE SERVICE
443/tcp open  https
MAC Address: 02:DF:10:BD:81:F6 (Unknown)

Nmap scan report for ip-10-5-30-117.ap-south-1.compute.internal (10.5.30.117)
Host is up (0.00s latency).
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
MAC Address: 02:4B:C4:A6:1D:9A (Unknown)

Nmap scan report for ip-10-5-18-1.ap-south-1.compute.internal (10.5.18.1)
Host is up (0.00s latency).
Not shown: 990 closed ports
PORT     STATE SERVICE
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
1025/tcp open  NFS-or-IIS
1026/tcp open  LSA-or-nterm
1027/tcp open  IIS
1028/tcp open  unknown
1041/tcp open  danf-ak2
1049/tcp open  td-postman
3389/tcp open  ms-wbt-server

Nmap done: 4096 IP addresses (9 hosts up) scanned in 27.53 seconds
```

From the above scan we can see that our target machine is _10.5.30.117_

[[Hack2.0/NMAP#Service version|NMAP Service scan and OS detection]]

```
PS C:\Users\Administrator> nmap -sV -O 10.5.30.117
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-07 03:56 Coordinated Universal Time
Nmap scan report for ip-10-5-30-117.ap-south-1.compute.internal (10.5.30.117)
Host is up (0.00s latency).
Not shown: 990 closed ports
PORT      STATE SERVICE            VERSION
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
3389/tcp  open  ssl/ms-wbt-server?
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49155/tcp open  msrpc              Microsoft Windows RPC
49165/tcp open  msrpc              Microsoft Windows RPC
49176/tcp open  msrpc              Microsoft Windows RPC
MAC Address: 02:4B:C4:A6:1D:9A (Unknown)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.91%E=4%D=7/7%OT=135%CT=1%CU=38105%PV=Y%DS=1%DC=D%G=Y%M=024BC4%T
OS:M=64A78D43%P=i686-pc-windows-windows)SEQ(SP=102%GCD=1%ISR=105%TI=I%CI=I%
OS:II=I%SS=S%TS=7)OPS(O1=M2301NW8ST11%O2=M2301NW8ST11%O3=M2301NW8NNT11%O4=M
OS:2301NW8ST11%O5=M2301NW8ST11%O6=M2301ST11)WIN(W1=2000%W2=2000%W3=2000%W4=
OS:2000%W5=2000%W6=2000)ECN(R=Y%DF=Y%T=80%W=2000%O=M2301NW8NNS%CC=Y%Q=)T1(R
OS:=Y%DF=Y%T=80%S=O%A=S+%F=AS%RD=0%Q=)T2(R=Y%DF=Y%T=80%W=0%S=Z%A=S%F=AR%O=%
OS:RD=0%Q=)T3(R=Y%DF=Y%T=80%W=0%S=Z%A=O%F=AR%O=%RD=0%Q=)T4(R=Y%DF=Y%T=80%W=
OS:0%S=A%A=O%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T
OS:6(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=80%W=0%S=Z%A=S+
OS:%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=80%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK
OS:=G%RUD=G)IE(R=Y%DFI=N%T=80%CD=Z)

Network Distance: 1 hop
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 78.31 seconds
```

[[Hack2.0/NMAP#Script scan|NMAP Default script scan]]

```
PS C:\Users\Administrator> nmap -sV -O -sC 10.5.30.117
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-07 03:59 Coordinated Universal Time
Nmap scan report for ip-10-5-30-117.ap-south-1.compute.internal (10.5.30.117)
Host is up (0.00s latency).
Not shown: 990 closed ports
PORT      STATE SERVICE            VERSION
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Windows Server 2012 R2 Standard 9600 microsoft-ds
3389/tcp  open  ssl/ms-wbt-server?
| rdp-ntlm-info:
|   Target_Name: WIN-OMCNBKR66MN
|   NetBIOS_Domain_Name: WIN-OMCNBKR66MN
|   NetBIOS_Computer_Name: WIN-OMCNBKR66MN
|   DNS_Domain_Name: WIN-OMCNBKR66MN
|   DNS_Computer_Name: WIN-OMCNBKR66MN
|   Product_Version: 6.3.9600
|_  System_Time: 2023-07-07T04:00:50+00:00
| ssl-cert: Subject: commonName=WIN-OMCNBKR66MN
| Not valid before: 2023-07-06T03:46:32
|_Not valid after:  2024-01-05T03:46:32
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49155/tcp open  msrpc              Microsoft Windows RPC
49165/tcp open  msrpc              Microsoft Windows RPC
49176/tcp open  msrpc              Microsoft Windows RPC
MAC Address: 02:4B:C4:A6:1D:9A (Unknown)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.91%E=4%D=7/7%OT=135%CT=1%CU=40868%PV=Y%DS=1%DC=D%G=Y%M=024BC4%T
OS:M=64A78E15%P=i686-pc-windows-windows)SEQ(SP=103%GCD=1%ISR=108%TI=I%CI=I%
OS:II=I%SS=S%TS=7)OPS(O1=M2301NW8ST11%O2=M2301NW8ST11%O3=M2301NW8NNT11%O4=M
OS:2301NW8ST11%O5=M2301NW8ST11%O6=M2301ST11)WIN(W1=2000%W2=2000%W3=2000%W4=
OS:2000%W5=2000%W6=2000)ECN(R=Y%DF=Y%T=80%W=2000%O=M2301NW8NNS%CC=Y%Q=)T1(R
OS:=Y%DF=Y%T=80%S=O%A=S+%F=AS%RD=0%Q=)T2(R=Y%DF=Y%T=80%W=0%S=Z%A=S%F=AR%O=%
OS:RD=0%Q=)T3(R=Y%DF=Y%T=80%W=0%S=Z%A=O%F=AR%O=%RD=0%Q=)T4(R=Y%DF=Y%T=80%W=
OS:0%S=A%A=O%F=R%O=%RD=0%Q=)T5(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T
OS:6(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=80%W=0%S=Z%A=S+
OS:%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%T=80%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK
OS:=G%RUD=G)IE(R=Y%DFI=N%T=80%CD=Z)

Network Distance: 1 hop
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: WIN-OMCNBKR66MN, NetBIOS user: <unknown>, NetBIOS MAC: 02:4b:c4:a6:1d:9a (unknown)
| smb-os-discovery:
|   OS: Windows Server 2012 R2 Standard 9600 (Windows Server 2012 R2 Standard 6.3)
|   OS CPE: cpe:/o:microsoft:windows_server_2012::-
|   Computer name: WIN-OMCNBKR66MN
|   NetBIOS computer name: WIN-OMCNBKR66MN\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2023-07-07T04:00:50+00:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   2.02:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2023-07-07T04:00:50
|_  start_date: 2023-07-07T03:46:28

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 113.24 seconds
```

Open a file explorer and you can go to the network and map a network to a specific drive.

![[Pasted image 20230707093441.png]]
![[Pasted image 20230707093557.png]]
In the popup we can enter the details as mentioned and map the network to a drive. In this case the server IP is 10.5.30.117 post that we can browse the drive we want to map. If incase we are asked for creds we use the ones we are provided.

![[Pasted image 20230707093902.png]]

![[Pasted image 20230707093932.png]]

### Command line approach :

#### Remove network drive:

```
PS C:\Users\Administrator> net use * /delete
You have these remote connections:

    Z:              \\10.5.30.117\c
                    \\10.5.30.117\IPC$
Continuing will cancel the connections.

Do you want to continue this operation? (Y/N) [N]: Y
There are open files and/or incomplete directory searches pending on the connection to Z:.

Is it OK to continue disconnecting and force them closed? (Y/N) [N]: Y
The command completed successfully.
```

#### Map a network drive

```
PS C:\Users\Administrator> net use Z: \\10.5.30.117\C$ smbserver_771 /user:administrator
The command completed successfully.
```

