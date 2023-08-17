### http2

A Kali GUI machine and a target machine running an IIS service are provided to you. The IP address of the target machine is provided in a text file named target placed on the Desktop of the Kali machine (/root/Desktop/target). 

Your task is to fingerprint the service using the tools available on the Kali machine and run Nmap scripts to enumerate the Windows target machine IIS service.

Objective: 

1. Identify IIS Server
2. Get webserver header details
3. Enumerated HTTP methods
4. Detect WebDAV configuration - etc.

[[ip]]

```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
4947: eth0@if4948: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:06 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.6/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
4949: eth1@if4950: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:0a:0c:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.10.12.2/24 brd 10.10.12.255 scope global eth1
       valid_lft forever preferred_lft forever
```

[[ping]]

```
root@attackdefense:~# cat /root/Desktop/target 
Target IP Address : 10.5.16.225
root@attackdefense:~# nmap 10.5.16.225
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-10 20:59 IST
Nmap scan report for 10.5.16.225
Host is up (0.0014s latency).
Not shown: 994 closed ports
PORT     STATE SERVICE
80/tcp   open  http
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
3306/tcp open  mysql
3389/tcp open  ms-wbt-server

Nmap done: 1 IP address (1 host up) scanned in 1.60 seconds
```

[[Hack2.0/NMAP#General Scan|NMAP general scan]]

```
root@attackdefense:~# nmap 10.5.16.225
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-10 20:59 IST
Nmap scan report for 10.5.16.225
Host is up (0.0014s latency).
Not shown: 994 closed ports
PORT     STATE SERVICE
80/tcp   open  http
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
3306/tcp open  mysql
3389/tcp open  ms-wbt-server

Nmap done: 1 IP address (1 host up) scanned in 1.60 seconds
```

```
root@attackdefense:~# nmap -p 80,135,139,445,3306,3389 -sV -O 10.5.16.225
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-10 21:00 IST
Nmap scan report for 10.5.16.225
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
Nmap done: 1 IP address (1 host up) scanned in 15.50 seconds
```

```
root@attackdefense:~# nmap -p80 --script http-enum 10.5.16.225
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-10 21:04 IST
Nmap scan report for 10.5.16.225
Host is up (0.0018s latency).

PORT   STATE SERVICE
80/tcp open  http
| http-enum: 
|   /content/: Potentially interesting folder
|   /downloads/: Potentially interesting folder
|_  /webdav/: Potentially interesting folder

Nmap done: 1 IP address (1 host up) scanned in 8.12 seconds
```

```
root@attackdefense:~# nmap -p80 --script http-headers 10.5.16.225
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-10 21:05 IST
Nmap scan report for 10.5.16.225
Host is up (0.0019s latency).

PORT   STATE SERVICE
80/tcp open  http
| http-headers: 
|   Cache-Control: private
|   Content-Type: text/html; charset=utf-8
|   Location: /Default.aspx
|   Server: Microsoft-IIS/10.0
|   Set-Cookie: ASP.NET_SessionId=e5p0dmbrda3nkx45mm2dgxog; path=/; HttpOnly; SameSite=Lax
|   X-AspNet-Version: 4.0.30319
|   Set-Cookie: Server=RE9UTkVUR09BVA==; path=/
|   X-XSS-Protection: 0
|   X-Powered-By: ASP.NET
|   Date: Mon, 10 Jul 2023 15:35:17 GMT
|   Connection: close
|   Content-Length: 130
|   
|_  (Request type: GET)

Nmap done: 1 IP address (1 host up) scanned in 0.78 seconds
```

```
root@attackdefense:~# nmap -p80 -sV --script http-methods --script-args http-methods.url-path=/webdav/ 10.5.16.225
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-10 21:08 IST
Nmap scan report for 10.5.16.225
Host is up (0.0018s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Microsoft IIS httpd 10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST COPY PROPFIND DELETE MOVE PROPPATCH MKCOL LOCK UNLOCK PUT
|   Potentially risky methods: TRACE COPY PROPFIND DELETE MOVE PROPPATCH MKCOL LOCK UNLOCK PUT
|_  Path tested: /webdav/
|_http-server-header: Microsoft-IIS/10.0
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.69 seconds
```

```
root@attackdefense:~# nmap -p80 -sV --script http-webdav-scan --script-args http-methods.url-path=/webdav/ 10.5.16.225
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-10 21:09 IST
Nmap scan report for 10.5.16.225
Host is up (0.0018s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-webdav-scan: 
|   Public Options: OPTIONS, TRACE, GET, HEAD, POST, PROPFIND, PROPPATCH, MKCOL, PUT, DELETE, COPY, MOVE, LOCK, UNLOCK
|   Server Type: Microsoft-IIS/10.0
|   WebDAV type: Unknown
|   Server Date: Mon, 10 Jul 2023 15:39:40 GMT
|_  Allowed Methods: OPTIONS, TRACE, GET, HEAD, POST, COPY, PROPFIND, LOCK, UNLOCK
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.76 seconds
```

