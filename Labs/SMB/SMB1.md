#### SMB1
`ris:PriceTag3` [[Servers and Services#SMB|SMB]] [[Hack2.0/NMAP|NMAP]] [[metasploit]]
`ris:Tools` [[Hack2.0/NMAP|NMAP]] [[metasploit]] [[nmblookup]] [[smbclient]] [[rpcclient]]

Questions:
1. Find the default tcp ports used by smbd.
2. Find the default udp ports used by nmbd.
3. What is the workgroup name of samba server?
4. Find the exact version of samba server by using appropriate nmap script.
5. Find the exact version of samba server by using smb_version metasploit module.
6. What is the NetBIOS computer name of samba server? Use appropriate nmap scripts.
7. Find the NetBIOS computer name of samba server using nmblookup
8. Using smbclient determine whether anonymous connection (null session)  is allowed on the samba server or not.
9. Using rpcclient determine whether anonymous connection (null session) is allowed on the samba server or not.

[[ip]]

```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
3467: eth0@if3468: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:05 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.5/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
3470: eth1@if3471: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:7c:69:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.124.105.2/24 brd 192.124.105.255 scope global eth1
       valid_lft forever preferred_lft forever
```

[[ping]]

```
root@attackdefense:~# ping -c 5 192.124.105.3
PING 192.124.105.3 (192.124.105.3) 56(84) bytes of data.
64 bytes from 192.124.105.3: icmp_seq=1 ttl=64 time=0.067 ms
64 bytes from 192.124.105.3: icmp_seq=2 ttl=64 time=0.047 ms
64 bytes from 192.124.105.3: icmp_seq=3 ttl=64 time=0.036 ms
64 bytes from 192.124.105.3: icmp_seq=4 ttl=64 time=0.051 ms
64 bytes from 192.124.105.3: icmp_seq=5 ttl=64 time=0.049 ms

--- 192.124.105.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 105ms
rtt min/avg/max/mdev = 0.036/0.050/0.067/0.010 ms
```

[[Hack2.0/NMAP#General Scan|NMAP General scan]]

```
root@attackdefense:~# nmap 192.124.105.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-07 18:32 UTC
Nmap scan report for target-1 (192.124.105.3)
Host is up (0.0000090s latency).
Not shown: 998 closed ports
PORT    STATE SERVICE
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
MAC Address: 02:42:C0:7C:69:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.18 seconds
```

[[Hack2.0/NMAP#Service version|NMAP Service versioning]]

```
root@attackdefense:~# nmap -p 139,445 -sV 192.124.105.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-07 18:33 UTC
Nmap scan report for target-1 (192.124.105.3)
Host is up (0.000033s latency).

PORT    STATE SERVICE     VERSION
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: RECONLABS)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: RECONLABS)
MAC Address: 02:42:C0:7C:69:03 (Unknown)
Service Info: Host: SAMBA-RECON

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.47 seconds
```

[[Hack2.0/NMAP#UDP Scan|NMAP UDP top ports scan]]

```
root@attackdefense:~# nmap -sU --top-ports 25 192.124.105.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-07 18:36 UTC
Nmap scan report for target-1 (192.124.105.3)
Host is up (0.000056s latency).

PORT      STATE         SERVICE
53/udp    closed        domain
67/udp    closed        dhcps
68/udp    closed        dhcpc
69/udp    closed        tftp
111/udp   closed        rpcbind
123/udp   closed        ntp
135/udp   closed        msrpc
137/udp   open          netbios-ns
138/udp   open|filtered netbios-dgm
139/udp   closed        netbios-ssn
161/udp   closed        snmp
162/udp   closed        snmptrap
445/udp   closed        microsoft-ds
500/udp   closed        isakmp
514/udp   closed        syslog
520/udp   closed        route
631/udp   closed        ipp
998/udp   closed        puparp
1434/udp  closed        ms-sql-m
1701/udp  closed        L2TP
1900/udp  closed        upnp
4500/udp  closed        nat-t-ike
5353/udp  closed        zeroconf
49152/udp closed        unknown
49154/udp closed        unknown
MAC Address: 02:42:C0:7C:69:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 19.42 seconds
```

Trying to list only open ports

```
root@attackdefense:~# nmap -sU --top-ports 25 192.124.105.3 --open
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-07 18:44 UTC
Nmap scan report for target-1 (192.124.105.3)
Host is up (0.000054s latency).
Not shown: 23 closed ports
PORT    STATE         SERVICE
137/udp open          netbios-ns
138/udp open|filtered netbios-dgm
MAC Address: 02:42:C0:7C:69:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 23.51 seconds
```

Service version of the UDP ports

```
root@attackdefense:~# nmap -p 137,138 -sU -sV 192.124.105.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-07 18:41 UTC
Nmap scan report for target-1 (192.124.105.3)
Host is up (0.000052s latency).

PORT    STATE         SERVICE     VERSION
137/udp open          netbios-ns  Samba nmbd netbios-ns (workgroup: RECONLABS)
138/udp open|filtered netbios-dgm
MAC Address: 02:42:C0:7C:69:03 (Unknown)
Service Info: Host: SAMBA-RECON

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 100.26 seconds
```

[[Hack2.0/NMAP#Specific script|NMAP specific script]]

```
root@attackdefense:~# nmap --script smb-os-discovery -p 139,445 192.124.105.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-07 18:49 UTC
Nmap scan report for target-1 (192.124.105.3)
Host is up (0.000042s latency).

PORT    STATE SERVICE
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
MAC Address: 02:42:C0:7C:69:03 (Unknown)

Host script results:
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: victim-1
|   NetBIOS computer name: SAMBA-RECON\x00
|   Domain name: \x00
|   FQDN: victim-1
|_  System time: 2023-07-07T18:49:59+00:00

Nmap done: 1 IP address (1 host up) scanned in 0.35 seconds
```

[[metasploit#Basic commands|Using msfconsole]]

```
msf5 > search smb_version

Matching Modules
================

   #  Name                               Disclosure Date  Rank    Check  Description
   -  ----                               ---------------  ----    -----  -----------
   1  auxiliary/scanner/smb/smb_version                   normal  Yes    SMB Version Detection


msf5 > use auxiliary/scanner/smb/smb_version
msf5 auxiliary(scanner/smb/smb_version) > options

Module options (auxiliary/scanner/smb/smb_version):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   RHOSTS                      yes       The target address range or CIDR identifier
   SMBDomain  .                no        The Windows domain to use for authentication
   SMBPass                     no        The password for the specified username
   SMBUser                     no        The username to authenticate as
   THREADS    1                yes       The number of concurrent threads

msf5 auxiliary(scanner/smb/smb_version) > set rhosts 192.124.105.3
rhosts => 192.124.105.3
msf5 auxiliary(scanner/smb/smb_version) > exploit

[*] 192.124.105.3:445     - Host could not be identified: Windows 6.1 (Samba 4.3.11-Ubuntu)
[*] 192.124.105.3:445     - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

[[nmblookup]]

```
root@attackdefense:~# nmblookup -A 192.124.105.3
Looking up status of 192.124.105.3
        SAMBA-RECON     <00> -         H <ACTIVE> 
        SAMBA-RECON     <03> -         H <ACTIVE> 
        SAMBA-RECON     <20> -         H <ACTIVE> 
        ..__MSBROWSE__. <01> - <GROUP> H <ACTIVE> 
        RECONLABS       <00> - <GROUP> H <ACTIVE> 
        RECONLABS       <1d> -         H <ACTIVE> 
        RECONLABS       <1e> - <GROUP> H <ACTIVE> 

        MAC Address = 00-00-00-00-00-00
```

[[smbclient#no password access|Access without password using smbclient]]

```
root@attackdefense:~# smbclient -L 192.124.105.3 -N

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

[[rpcclient#login without password|login using rpcclient]]

```
root@attackdefense:~# rpcclient -U "" -N 192.124.105.3
rpcclient $> ls
```
