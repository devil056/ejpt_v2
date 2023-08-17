### SSH1

`ris:PriceTag3` [[Servers and Services#SSH|SSH]]
`ris:Tools` [[Hack2.0/NMAP|NMAP]]

1. What is the version of SSH server.
2. Fetch the banner using netcat and check the version of SSH server.
3. Fetch pre-login SSH banner.
4. How many “encryption_algorithms” are supported by the SSH server.
5. What is the ssh-rsa host key being used by the SSH server.
6. Which authentication method is being used by the SSH server for user “student”.
7. Which authentication method is being used by the SSH server for user “admin”.
8. Fetch the flag from /home/student/FLAG by using nmap ssh-run script.

[[ip]]

```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
2710: eth0@if2711: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:04 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.4/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
2713: eth1@if2714: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:db:06:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.219.6.2/24 brd 192.219.6.255 scope global eth1
       valid_lft forever preferred_lft forever
```

[[ping]]

```
root@attackdefense:~# ping -c 5 192.219.6.3
PING 192.219.6.3 (192.219.6.3) 56(84) bytes of data.
64 bytes from 192.219.6.3: icmp_seq=1 ttl=64 time=0.088 ms
64 bytes from 192.219.6.3: icmp_seq=2 ttl=64 time=0.023 ms
64 bytes from 192.219.6.3: icmp_seq=3 ttl=64 time=0.022 ms
64 bytes from 192.219.6.3: icmp_seq=4 ttl=64 time=0.022 ms
64 bytes from 192.219.6.3: icmp_seq=5 ttl=64 time=0.019 ms

--- 192.219.6.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 80ms
rtt min/avg/max/mdev = 0.019/0.034/0.088/0.027 ms
```

[[Hack2.0/NMAP#General Scan|NMAP scan]]

```
root@attackdefense:~# nmap 192.219.6.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 10:05 UTC
Nmap scan report for target-1 (192.219.6.3)
Host is up (0.000010s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
MAC Address: 02:42:C0:DB:06:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.23 seconds
```

[[Hack2.0/NMAP#Service version|NMAP service scan]]

```
root@attackdefense:~# nmap -p22 -sV -O 192.219.6.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 10:06 UTC
Nmap scan report for target-1 (192.219.6.3)
Host is up (0.000030s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)
MAC Address: 02:42:C0:DB:06:03 (Unknown)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 2.6.32 (96%), Linux 3.2 - 4.9 (96%), Linux 2.6.32 - 3.10 (96%), Linux 3.4 - 3.10 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), Synology DiskStation Manager 5.2-5644 (94%), Netgear RAIDiator 4.2.28 (94%), Linux 2.6.32 - 2.6.35 (94%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 4.10 seconds
```

[[ssh#basic login|ssh login]]

```
root@attackdefense:~# ssh root@192.219.6.3
The authenticity of host '192.219.6.3 (192.219.6.3)' can't be established.
ECDSA key fingerprint is SHA256:dxlBXgBb0Iv5/LmemZ2Eikb5+GLl9CSLf/B854fUeV8.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.219.6.3' (ECDSA) to the list of known hosts.
Welcome to attack defense ssh recon lab!!
root@192.219.6.3's password: 
Permission denied, please try again.
root@192.219.6.3's password: 
```

[[netcat#basic connect| nc connect]]

```
root@attackdefense:~# nc 192.219.6.3 22
SSH-2.0-OpenSSH_7.2p2 Ubuntu-4ubuntu2.6

Protocol mismatch.
```

[[Hack2.0/NMAP#Specific script|NMAP checking using script]]

```
root@attackdefense:~# nmap --script ssh2-enum-algos 192.219.6.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 10:17 UTC
Nmap scan report for target-1 (192.219.6.3)
Host is up (0.0000090s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
| ssh2-enum-algos: 
|   kex_algorithms: (6)
|       curve25519-sha256@libssh.org
|       ecdh-sha2-nistp256
|       ecdh-sha2-nistp384
|       ecdh-sha2-nistp521
|       diffie-hellman-group-exchange-sha256
|       diffie-hellman-group14-sha1
|   server_host_key_algorithms: (5)
|       ssh-rsa
|       rsa-sha2-512
|       rsa-sha2-256
|       ecdsa-sha2-nistp256
|       ssh-ed25519
|   encryption_algorithms: (6)
|       chacha20-poly1305@openssh.com
|       aes128-ctr
|       aes192-ctr
|       aes256-ctr
|       aes128-gcm@openssh.com
|       aes256-gcm@openssh.com
|   mac_algorithms: (10)
|       umac-64-etm@openssh.com
|       umac-128-etm@openssh.com
|       hmac-sha2-256-etm@openssh.com
|       hmac-sha2-512-etm@openssh.com
|       hmac-sha1-etm@openssh.com
|       umac-64@openssh.com
|       umac-128@openssh.com
|       hmac-sha2-256
|       hmac-sha2-512
|       hmac-sha1
|   compression_algorithms: (2)
|       none
|_      zlib@openssh.com
MAC Address: 02:42:C0:DB:06:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.37 seconds
```


```
root@attackdefense:~# nmap --script ssh-hostkey --script-args ssh_hostkey=full 192.219.6.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 10:19 UTC
Nmap scan report for target-1 (192.219.6.3)
Host is up (0.0000090s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
| ssh-hostkey: 
|   ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC1fkJK7F8yxf3vewEcLYHljBnKTAiRqzFxkFo6lqyew73ATL2Abyh6at/oOmBSlPI90rtAMA6jQGJ+0HlHgf7mkjz5+CBo9j2VPu1bejYtcxpqpHcL5Bp12wgey1zup74fgd+yOzILjtgbnDOw1+HSkXqN79d+4BnK0QF6T9YnkHvBhZyjzIDmjonDy92yVBAIoB6Rdp0w7nzFz3aN9gzB5MW/nSmgc4qp7R6xtzGaqZKp1H3W3McZO3RELjGzvHOdRkAKL7n2kyVAraSUrR0Oo5m5e/sXrITYi9y0X6p2PTUfYiYvgkv/3xUF+5YDDA33AJvv8BblnRcRRZ74BxaD
|   ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBB0cJ/kSOXBWVIBA2QH4UB6r7nFL5l7FwHubbSZ9dIs2JSmn/oIgvvQvxmI5YJxkdxRkQlF01KLDmVgESYXyDT4=
|_  ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIKuZlCFfTgeaMC79zla20ZM2q64mjqWhKPw/2UzyQ2W/
MAC Address: 02:42:C0:DB:06:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.45 seconds
```

```
root@attackdefense:~# nmap --script ssh-auth-methods --script-args ssh.user=student 192.219.6.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 10:20 UTC
Nmap scan report for target-1 (192.219.6.3)
Host is up (0.000010s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
| ssh-auth-methods: 
|_  Supported authentication methods: none_auth
MAC Address: 02:42:C0:DB:06:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.37 seconds
```

```
root@attackdefense:~# nmap --script ssh-auth-methods --script-args ssh.user=admin 192.219.6.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 10:21 UTC
Nmap scan report for target-1 (192.219.6.3)
Host is up (0.000010s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
| ssh-auth-methods: 
|   Supported authentication methods: 
|     publickey
|_    password
MAC Address: 02:42:C0:DB:06:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 2.18 seconds
```

```
root@attackdefense:~# ssh student@192.219.6.3
Welcome to attack defense ssh recon lab!!
Welcome to Ubuntu 16.04.5 LTS (GNU/Linux 5.4.0-153-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

student@victim-1:~$ ls
FLAG
student@victim-1:~$ cat FLAG 
e1e3c0c9d409f594afdb18fe9ce0ffec
```

```
root@attackdefense:~# nmap -p22 --script ssh-run --script-args ssh-run.cmd='ls -l',ssh-run.username=student,ssh-run.password='' 192.219.6.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 10:31 UTC
NSE: [ssh-run] Authenticated
NSE: [ssh-run] Running command: ls -l
NSE: [ssh-run] Output of command: total 4
-rw-r--r-- 1 root root 33 Nov 22  2018 FLAG

Nmap scan report for target-1 (192.219.6.3)
Host is up (0.000051s latency).

PORT   STATE SERVICE
22/tcp open  ssh
| ssh-run: 
|   output: 
|     total 4\x0D
|_-rw-r--r-- 1 root root 33 Nov 22  2018 FLAG\x0D
MAC Address: 02:42:C0:DB:06:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.40 seconds
root@attackdefense:~# nmap -p22 --script ssh-run --script-args ssh-run.cmd='cat FLAG',ssh-run.username=student,ssh-run.password='' 192.219.6.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-10 10:31 UTC
NSE: [ssh-run] Authenticated
NSE: [ssh-run] Running command: cat FLAG
NSE: [ssh-run] Output of command: e1e3c0c9d409f594afdb18fe9ce0ffec

Nmap scan report for target-1 (192.219.6.3)
Host is up (0.000058s latency).

PORT   STATE SERVICE
22/tcp open  ssh
| ssh-run: 
|   output: 
|_    e1e3c0c9d409f594afdb18fe9ce0ffec\x0D
MAC Address: 02:42:C0:DB:06:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.41 seconds
```

