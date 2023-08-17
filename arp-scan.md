#### arp-scan

In this scan we specify the interface and the range of IP addresses we would like to find. By doing so what actually happens is that we send out requests to the devices to check which system holds the IP address currently we are searching for, if they do please respond back to our attack machine. Once we get responses for all the IP addresses we are searching for we can check in Wireshark statistics all the IP addresses we got.

Before we run the command to start arp-scan we need to launch the wireshark. And then we can start the arp-scan.

![[Pasted image 20230705130018.png]]

```
┌──(kali㉿kali)-[~]
└─$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:53:0c:ba brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.5/24 brd 10.0.2.255 scope global dynamic noprefixroute eth0
       valid_lft 340sec preferred_lft 340sec
    inet6 fe80::c64d:5ad5:101c:6831/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
                                                                                                                                                                          
┌──(kali㉿kali)-[~]
└─$ sudo arp-scan -I eth0 -g 10.0.2.5/24
[sudo] password for kali: 
Interface: eth0, type: EN10MB, MAC: 08:00:27:53:0c:ba, IPv4: 10.0.2.5
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
WARNING: host part of 10.0.2.5/24 is non-zero
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
10.0.2.1        52:54:00:12:35:00       (Unknown: locally administered)
10.0.2.2        52:54:00:12:35:00       (Unknown: locally administered)
10.0.2.3        08:00:27:ad:31:57       (Unknown)

3 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.867 seconds (137.12 hosts/sec). 3 responded
```

By doing so we can see the available devices on the statistics tab and their MAC addresses and IP address that the devices hold, all at once.