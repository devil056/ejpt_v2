### http1


A Kali GUI machine and a target machine running an IIS service are provided to you. The IP address of the target machine is provided in a text file named target placed on the Desktop of the Kali machine (/root/Desktop/target). 

Your task and objective are to fingerprint the service using the tools available on the Kali machine (WhatWeb, Dirb, Browsh) and extract target server information.

[[ip]]

```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
4936: eth0@if4937: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:06 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.6/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
4938: eth1@if4939: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:0a:0c:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.10.12.2/24 brd 10.10.12.255 scope global eth1
       valid_lft forever preferred_lft forever
```

[[ping]]

```
root@attackdefense:~# cat /root/Desktop/target 
Target IP Address : 10.5.31.81
root@attackdefense:~# ping -c 5 10.5.31.81
PING 10.5.31.81 (10.5.31.81) 56(84) bytes of data.
64 bytes from 10.5.31.81: icmp_seq=1 ttl=125 time=2.42 ms
64 bytes from 10.5.31.81: icmp_seq=2 ttl=125 time=1.61 ms
64 bytes from 10.5.31.81: icmp_seq=3 ttl=125 time=1.79 ms
64 bytes from 10.5.31.81: icmp_seq=4 ttl=125 time=1.59 ms
64 bytes from 10.5.31.81: icmp_seq=5 ttl=125 time=1.64 ms

--- 10.5.31.81 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4007ms
rtt min/avg/max/mdev = 1.593/1.811/2.419/0.311 ms
```

[[Hack2.0/NMAP#General Scan|NMAP basic scan]]

```
root@attackdefense:~# nmap 10.5.31.81
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-10 20:36 IST
Nmap scan report for 10.5.31.81
Host is up (0.0016s latency).
Not shown: 994 closed ports
PORT     STATE SERVICE
80/tcp   open  http
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
3306/tcp open  mysql
3389/tcp open  ms-wbt-server

Nmap done: 1 IP address (1 host up) scanned in 1.65 seconds
```

```
root@attackdefense:~# nmap -p 80,135,139,445,3306,3389 -sV -O 10.5.31.81
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-10 20:37 IST
Nmap scan report for 10.5.31.81
Host is up (0.0016s latency).

PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft IIS httpd 10.0
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
3306/tcp open  mysql         MySQL (unauthorized)
3389/tcp open  ms-wbt-server Microsoft Terminal Services
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Microsoft Windows 10 1709 - 1909 (93%), Microsoft Windows Server 2012 (93%), Microsoft Windows Vista SP1 (92%), Microsoft Windows Longhorn (92%), Microsoft Windows 10 1709 - 1803 (91%), Microsoft Windows 10 1809 - 1909 (91%), Microsoft Windows Server 2012 R2 (91%), Microsoft Windows Server 2012 R2 Update 1 (91%), Microsoft Windows Server 2016 build 10586 - 14393 (91%), Microsoft Windows 7, Windows Server 2012, or Windows 8.1 Update 1 (91%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 3 hops
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 16.23 seconds
```

![[Pasted image 20230710203906.png]]

[[Hack2.0/Commands2.0/whatweb|whatweb]]

```
root@attackdefense:~# whatweb 10.5.31.81
Ignoring eventmachine-1.3.0.dev.1 because its extensions are not built. Try: gem pristine eventmachine --version 1.3.0.dev.1
Ignoring fxruby-1.6.29 because its extensions are not built. Try: gem pristine fxruby --version 1.6.29
http://10.5.31.81 [302 Found] ASP_NET[4.0.30319], Cookies[ASP.NET_SessionId,Server], Country[RESERVED][ZZ], HTTPServer[Microsoft-IIS/10.0], HttpOnly[ASP.NET_SessionId], IP[10.5.31.81], Microsoft-IIS[10.0], RedirectLocation[/Default.aspx], Title[Object moved], X-Powered-By[ASP.NET], X-XSS-Protection[0]
http://10.5.31.81/Default.aspx [302 Found] ASP_NET[4.0.30319], Cookies[ASP.NET_SessionId,Server], Country[RESERVED][ZZ], HTTPServer[Microsoft-IIS/10.0], HttpOnly[ASP.NET_SessionId], IP[10.5.31.81], Microsoft-IIS[10.0], RedirectLocation[/Default.aspx], Title[Object moved], X-Powered-By[ASP.NET], X-XSS-Protection[0]
```

[[Hack2.0/Commands2.0/http|http]]

```
root@attackdefense:~# http 10.5.31.81
HTTP/1.1 302 Found
Cache-Control: private
Content-Length: 130
Content-Type: text/html; charset=utf-8
Date: Mon, 10 Jul 2023 15:11:57 GMT
Location: /Default.aspx
Server: Microsoft-IIS/10.0
Set-Cookie: ASP.NET_SessionId=cq2rbfx4p4vzq2xb5o1hbxm4; path=/; HttpOnly; SameSite=Lax
Set-Cookie: Server=RE9UTkVUR09BVA==; path=/
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
X-XSS-Protection: 0

<html><head><title>Object moved</title></head><body>
<h2>Object moved to <a href="/Default.aspx">here</a>.</h2>
</body></html>

```

[[Hack2.0/Commands2.0/dirb|dirb]]

```
root@attackdefense:~# dirb -h

-----------------
DIRB v2.22    
By The Dark Raver
-----------------


(!) FATAL: Invalid URL format: -h/
    (Use: "http://host/" or "https://host/" for SSL)
root@attackdefense:~# dirb http://10.5.31.81

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Mon Jul 10 20:46:31 2023
URL_BASE: http://10.5.31.81/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.5.31.81/ ----
==> DIRECTORY: http://10.5.31.81/app_themes/                                                                                                                                                                                                                                   
==> DIRECTORY: http://10.5.31.81/aspnet_client/                                                                                                                                                                                                                                
==> DIRECTORY: http://10.5.31.81/configuration/                                                                                                                                                                                                                                
==> DIRECTORY: http://10.5.31.81/content/                                                                                                                                                                                                                                      
==> DIRECTORY: http://10.5.31.81/Content/                                                                                                                                                                                                                                      
==> DIRECTORY: http://10.5.31.81/downloads/                                                                                                                                                                                                                                    
==> DIRECTORY: http://10.5.31.81/Downloads/                                                                                                                                                                                                                                    
==> DIRECTORY: http://10.5.31.81/resources/                                                                                                                                                                                                                                    
==> DIRECTORY: http://10.5.31.81/Resources/
```

[[Hack2.0/browsh|browsh]]

![[Pasted image 20230710205415.png]]

