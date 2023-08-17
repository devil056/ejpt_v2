### SMB3
`ris:PriceTag3` [[Servers and Services#SMB|SMB]]
`ris:Tools` [[metasploit]] [[Hack2.0/NMAP|NMAP]] [[enum4linux]] [[rpcclient]] [[smbclient]]

Questions
1. List all available shares on the samba server using Nmap script.
2. List all available shares on the samba server using smb_enumshares Metasploit module.
3. List all available shares on the samba server using enum4Linux.
4. List all available shares on the samba server using smbclient.
5. Find domain groups that exist on the samba server by using enum4Linux.
6. Find domain groups that exist on the samba server by using rpcclient.
7. Is samba server configured for printing?
8. How many directories are present inside share “public”?
9. Fetch the flag from samba server.

[[ip]]

```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
3936: eth0@if3937: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:08 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.8/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
3939: eth1@if3940: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:39:a8:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.57.168.2/24 brd 192.57.168.255 scope global eth1
       valid_lft forever preferred_lft forever
```

[[ping]]

```
root@attackdefense:~# ping -c 5 192.57.168.3
PING 192.57.168.3 (192.57.168.3) 56(84) bytes of data.
64 bytes from 192.57.168.3: icmp_seq=1 ttl=64 time=0.104 ms
64 bytes from 192.57.168.3: icmp_seq=2 ttl=64 time=0.035 ms
64 bytes from 192.57.168.3: icmp_seq=3 ttl=64 time=0.038 ms
64 bytes from 192.57.168.3: icmp_seq=4 ttl=64 time=0.074 ms
64 bytes from 192.57.168.3: icmp_seq=5 ttl=64 time=0.043 ms

--- 192.57.168.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 76ms
rtt min/avg/max/mdev = 0.035/0.058/0.104/0.028 ms
```

[[Hack2.0/NMAP#General Scan|NMAP general scan]]

```
root@attackdefense:~# nmap 192.57.168.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-08 17:03 UTC
Nmap scan report for target-1 (192.57.168.3)
Host is up (0.000010s latency).
Not shown: 998 closed ports
PORT    STATE SERVICE
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
MAC Address: 02:42:C0:39:A8:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.17 seconds
```

[[Hack2.0/NMAP#Specific script|NMAP checking the shares]]

```
root@attackdefense:~# nmap -p 139,445 --script smb-enum-shares 192.57.168.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-08 17:08 UTC
Nmap scan report for target-1 (192.57.168.3)
Host is up (0.000028s latency).

PORT    STATE SERVICE
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
MAC Address: 02:42:C0:39:A8:03 (Unknown)

Host script results:
| smb-enum-shares: 
|   account_used: guest
|   \\192.57.168.3\IPC$: 
|     Type: STYPE_IPC_HIDDEN
|     Comment: IPC Service (samba.recon.lab)
|     Users: 1
|     Max Users: <unlimited>
|     Path: C:\tmp
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\192.57.168.3\aisha: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\samba\aisha
|     Anonymous access: <none>
|     Current user access: <none>
|   \\192.57.168.3\emma: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\samba\emma
|     Anonymous access: <none>
|     Current user access: <none>
|   \\192.57.168.3\everyone: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\samba\everyone
|     Anonymous access: <none>
|     Current user access: <none>
|   \\192.57.168.3\john: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\samba\john
|     Anonymous access: <none>
|     Current user access: <none>
|   \\192.57.168.3\public: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\samba\public
|     Anonymous access: READ/WRITE
|_    Current user access: READ/WRITE

Nmap done: 1 IP address (1 host up) scanned in 0.73 seconds
```

[[metasploit]]

```
msf5 > search smb_enumshares

Matching Modules
================

   #  Name                                  Disclosure Date  Rank    Check  Description
   -  ----                                  ---------------  ----    -----  -----------
   1  auxiliary/scanner/smb/smb_enumshares                   normal  Yes    SMB Share Enumeration


msf5 > use auxiliary/scanner/smb/smb_enumshares
msf5 auxiliary(scanner/smb/smb_enumshares) > options

Module options (auxiliary/scanner/smb/smb_enumshares):

   Name            Current Setting  Required  Description
   ----            ---------------  --------  -----------
   LogSpider       3                no        0 = disabled, 1 = CSV, 2 = table (txt), 3 = one liner (txt) (Accepted: 0, 1, 2, 3)
   MaxDepth        999              yes       Max number of subdirectories to spider
   RHOSTS                           yes       The target address range or CIDR identifier
   SMBDomain       .                no        The Windows domain to use for authentication
   SMBPass                          no        The password for the specified username
   SMBUser                          no        The username to authenticate as
   ShowFiles       false            yes       Show detailed information when spidering
   SpiderProfiles  true             no        Spider only user profiles when share = C$
   SpiderShares    false            no        Spider shares recursively
   THREADS         1                yes       The number of concurrent threads

msf5 auxiliary(scanner/smb/smb_enumshares) > set rhosts 192.57.168.3
rhosts => 192.57.168.3
msf5 auxiliary(scanner/smb/smb_enumshares) > exploit

[+] 192.57.168.3:139      - public - (DS) 
[+] 192.57.168.3:139      - john - (DS) 
[+] 192.57.168.3:139      - aisha - (DS) 
[+] 192.57.168.3:139      - emma - (DS) 
[+] 192.57.168.3:139      - everyone - (DS) 
[+] 192.57.168.3:139      - IPC$ - (I) IPC Service (samba.recon.lab)
[*] 192.57.168.3:         - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/smb/smb_enumshares) >
```

[[enum4linux]]

```
root@attackdefense:~# enum4linux -S 192.57.168.3
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Sat Jul  8 17:12:43 2023

 ========================== 
|    Target Information    |
 ========================== 
Target ........... 192.57.168.3
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ==================================================== 
|    Enumerating Workgroup/Domain on 192.57.168.3    |
 ==================================================== 
[+] Got domain/workgroup name: RECONLABS

 ===================================== 
|    Session Check on 192.57.168.3    |
 ===================================== 
[+] Server 192.57.168.3 allows sessions using username '', password ''

 =========================================== 
|    Getting domain SID for 192.57.168.3    |
 =========================================== 
Domain Name: RECONLABS
Domain Sid: (NULL SID)
[+] Can't determine if host is part of domain or part of a workgroup

 ========================================= 
|    Share Enumeration on 192.57.168.3    |
 ========================================= 

        Sharename       Type      Comment
        ---------       ----      -------
        public          Disk      
        john            Disk      
        aisha           Disk      
        emma            Disk      
        everyone        Disk      
        IPC$            IPC       IPC Service (samba.recon.lab)
Reconnecting with SMB1 for workgroup listing.

        Server               Comment
        ---------            -------

        Workgroup            Master
        ---------            -------
        RECONLABS            SAMBA-RECON

[+] Attempting to map shares on 192.57.168.3
//192.57.168.3/public   Mapping: OK, Listing: OK
//192.57.168.3/john     Mapping: DENIED, Listing: N/A
//192.57.168.3/aisha    Mapping: DENIED, Listing: N/A
//192.57.168.3/emma     Mapping: DENIED, Listing: N/A
//192.57.168.3/everyone Mapping: DENIED, Listing: N/A
//192.57.168.3/IPC$     [E] Can't understand response:
NT_STATUS_OBJECT_NAME_NOT_FOUND listing \*
enum4linux complete on Sat Jul  8 17:12:44 2023
```

```
root@attackdefense:~# enum4linux -G 192.57.168.3
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Sat Jul  8 17:16:14 2023

 ========================== 
|    Target Information    |
 ========================== 
Target ........... 192.57.168.3
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ==================================================== 
|    Enumerating Workgroup/Domain on 192.57.168.3    |
 ==================================================== 
[+] Got domain/workgroup name: RECONLABS

 ===================================== 
|    Session Check on 192.57.168.3    |
 ===================================== 
[+] Server 192.57.168.3 allows sessions using username '', password ''

 =========================================== 
|    Getting domain SID for 192.57.168.3    |
 =========================================== 
Domain Name: RECONLABS
Domain Sid: (NULL SID)
[+] Can't determine if host is part of domain or part of a workgroup

 ============================== 
|    Groups on 192.57.168.3    |
 ============================== 

[+] Getting builtin groups:

[+] Getting builtin group memberships:

[+] Getting local groups:
group:[Testing] rid:[0x3f0]

[+] Getting local group memberships:

[+] Getting domain groups:
group:[Maintainer] rid:[0x3ee]
group:[Reserved] rid:[0x3ef]

[+] Getting domain group memberships:
enum4linux complete on Sat Jul  8 17:16:14 2023
```

[[rpcclient|finding domain groups using rpcclient]]

```
root@attackdefense:~# rpcclient -U "" -N 192.57.168.3
rpcclient $>enumdomgroups
group:[Maintainer] rid:[0x3ee]
group:[Reserved] rid:[0x3ef]
```

[[enum4linux|checking for printer config using enum4linux]]

```
root@attackdefense:~# enum4linux -i 192.57.168.3
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Sat Jul  8 17:20:18 2023

 ========================== 
|    Target Information    |
 ========================== 
Target ........... 192.57.168.3
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ==================================================== 
|    Enumerating Workgroup/Domain on 192.57.168.3    |
 ==================================================== 
[+] Got domain/workgroup name: RECONLABS

 ===================================== 
|    Session Check on 192.57.168.3    |
 ===================================== 
[+] Server 192.57.168.3 allows sessions using username '', password ''

 =========================================== 
|    Getting domain SID for 192.57.168.3    |
 =========================================== 
Domain Name: RECONLABS
Domain Sid: (NULL SID)
[+] Can't determine if host is part of domain or part of a workgroup

 ============================================= 
|    Getting printer info for 192.57.168.3    |
 ============================================= 
No printers returned.


enum4linux complete on Sat Jul  8 17:20:19 2023
```

[[smbclient|listing shares using the smbclient]]

```
root@attackdefense:~# smbclient -L //192.57.168.3/ -N

        Sharename       Type      Comment
        ---------       ----      -------
        public          Disk      
        john            Disk      
        aisha           Disk      
        emma            Disk      
        everyone        Disk      
        IPC$            IPC       IPC Service (samba.recon.lab)
Reconnecting with SMB1 for workgroup listing.

        Server               Comment
        ---------            -------

        Workgroup            Master
        ---------            -------
        RECONLABS            SAMBA-RECON
```

[[smbclient|getting flag and listing dir in Public]]

```
root@attackdefense:~# smbclient -L //192.57.168.3/ -N

        Sharename       Type      Comment
        ---------       ----      -------
        public          Disk      
        john            Disk      
        aisha           Disk      
        emma            Disk      
        everyone        Disk      
        IPC$            IPC       IPC Service (samba.recon.lab)
Reconnecting with SMB1 for workgroup listing.

        Server               Comment
        ---------            -------

        Workgroup            Master
        ---------            -------
        RECONLABS            SAMBA-RECON
root@attackdefense:~# smbclient //192.57.168.3/Public -N
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Sat Jul  8 17:08:01 2023
  ..                                  D        0  Tue Nov 27 13:36:13 2018
  dev                                 D        0  Tue Nov 27 13:36:13 2018
  secret                              D        0  Tue Nov 27 13:36:13 2018

                1981084628 blocks of size 1024. 223606864 blocks available
smb: \> cd secret\
smb: \secret\> ls
  .                                   D        0  Tue Nov 27 13:36:13 2018
  ..                                  D        0  Sat Jul  8 17:08:01 2023
  flag                                N       33  Tue Nov 27 13:36:13 2018

                1981084628 blocks of size 1024. 223606784 blocks available
smb: \secret\> cat flag
cat: command not found
smb: \secret\> get flag 
getting file \secret\flag of size 33 as flag (32.2 KiloBytes/sec) (average 32.2 KiloBytes/sec)
smb: \secret\> exit
root@attackdefense:~# ls
README  flag  tools  wordlists
root@attackdefense:~# cat flag 
03ddb97933e716f5057a18632badb3b4
```

