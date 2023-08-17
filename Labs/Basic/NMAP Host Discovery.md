Your task is to discover available live hosts and their open ports using Nmap and identify the running services and applications.

`ris:PriceTag3` [[Hack2.0/NMAP|NMAP]] [[ping]]

Let us go !!!

Target IP : 10.5.25.152

Step 1 : Find your IP

```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
24918: eth0@if24919: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:05 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.5/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
24920: eth1@if24921: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:0a:13:03 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.10.19.3/24 brd 10.10.19.255 scope global eth1
       valid_lft forever preferred_lft forever

```

Step 2 : Check if the target system is active you can use ping for that.

```
root@attackdefense:~# ping -c 5 10.5.25.152
PING 10.5.25.152 (10.5.25.152) 56(84) bytes of data.

--- 10.5.25.152 ping statistics ---
5 packets transmitted, 0 received, 100% packet loss, time 4092ms
```

Okay I did not get any response, like we discussed earlier from the ping command if the device is working on windows it will not respond. So we will go ahead and check the same with nmap as well.

Step 3 : NMAP Scan assuming it to be windows

```
root@attackdefense:~# nmap 10.5.25.152
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-02 01:18 IST
Note: Host seems down. If it is really up, but blocking our ping probes, try -Pn
Nmap done: 1 IP address (0 hosts up) scanned in 3.11 seconds
```

Even if we do not provide necessary options we will be reminded as above so there is no chance of missing on it. Now we will run the same command but with the option we got from above.

```
root@attackdefense:~# nmap -Pn 10.5.25.152
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-02 01:20 IST
Nmap scan report for 10.5.25.152
Host is up (0.0025s latency).
Not shown: 993 filtered ports
PORT      STATE SERVICE
80/tcp    open  http
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
3389/tcp  open  ms-wbt-server
49154/tcp open  unknown
49155/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 4.65 seconds
```


```
root@attackdefense:~# nmap -p 80,135,139,445,3389,49154,49155 -Pn -sV -O 10.5.25.152
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-02 01:27 IST
Nmap scan report for 10.5.25.152
Host is up (0.0026s latency).

PORT      STATE SERVICE            VERSION
80/tcp    open  http               HttpFileServer httpd 2.3
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
3389/tcp  open  ssl/ms-wbt-server?
49154/tcp open  msrpc              Microsoft Windows RPC
49155/tcp open  msrpc              Microsoft Windows RPC
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Microsoft Windows 2012
OS CPE: cpe:/o:microsoft:windows_server_2012
OS details: Microsoft Windows Server 2012
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 77.32 seconds
```


```
root@attackdefense:~# nmap -p 80,135,139,445,3389,49154,49155 -Pn -sV -O -sC 10.5.25.152
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-02 01:42 IST
Stats: 0:00:44 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 57.14% done; ETC: 01:43 (0:00:32 remaining)
Nmap scan report for 10.5.25.152
Host is up (0.0019s latency).

PORT      STATE SERVICE            VERSION
80/tcp    open  http               HttpFileServer httpd 2.3
|_http-server-header: HFS 2.3
|_http-title: HFS /
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
3389/tcp  open  ssl/ms-wbt-server?
| ssl-cert: Subject: commonName=http-server
| Not valid before: 2023-06-30T19:43:38
|_Not valid after:  2023-12-30T19:43:38
49154/tcp open  msrpc              Microsoft Windows RPC
49155/tcp open  msrpc              Microsoft Windows RPC
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Microsoft Windows 2012
OS CPE: cpe:/o:microsoft:windows_server_2012
OS details: Microsoft Windows Server 2012
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2023-07-02 01:43:58
|_  start_date: 2023-07-02 01:13:33

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 117.29 seconds
```
This will be response for the task provided to us.