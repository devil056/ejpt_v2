Port scanning is the goal of this lab

`ris:Tools` [[Hack2.0/NMAP|NMAP]]

[[ip]]

```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
1407: eth0@if1408: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:04 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.4/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
1410: eth1@if1411: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:ab:5f:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.171.95.2/24 brd 192.171.95.255 scope global eth1
       valid_lft forever preferred_lft forever
```

[[ping]]

```
root@attackdefense:~# ping -c 5 192.171.95.3
PING 192.171.95.3 (192.171.95.3) 56(84) bytes of data.
64 bytes from 192.171.95.3: icmp_seq=1 ttl=64 time=0.082 ms
64 bytes from 192.171.95.3: icmp_seq=2 ttl=64 time=0.039 ms
64 bytes from 192.171.95.3: icmp_seq=3 ttl=64 time=0.025 ms
64 bytes from 192.171.95.3: icmp_seq=4 ttl=64 time=0.026 ms
64 bytes from 192.171.95.3: icmp_seq=5 ttl=64 time=0.028 ms

--- 192.171.95.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 84ms
rtt min/avg/max/mdev = 0.025/0.040/0.082/0.021 ms
```

[[Hack2.0/NMAP#General Scan|NMAP general scan]]

```
root@attackdefense:~# nmap 192.171.95.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-06 15:15 UTC
Nmap scan report for target-1 (192.171.95.3)
Host is up (0.0000090s latency).
All 1000 scanned ports on target-1 (192.171.95.3) are closed
MAC Address: 02:42:C0:AB:5F:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.23 seconds
```

[[Hack2.0/NMAP#scan all ports|NMAP All port scan]]

```
root@attackdefense:~# nmap -p- 192.171.95.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-06 15:15 UTC
Nmap scan report for target-1 (192.171.95.3)
Host is up (0.0000090s latency).
All 65535 scanned ports on target-1 (192.171.95.3) are closed
MAC Address: 02:42:C0:AB:5F:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 1.09 seconds
```

[[Hack2.0/NMAP#UDP Scan|NMAP UDP Scan]]
 
```
root@attackdefense:~# nmap -sU 192.171.95.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-06 15:15 UTC
Nmap scan report for target-1 (192.171.95.3)
Host is up (0.000034s latency).
Not shown: 999 closed ports
PORT    STATE SERVICE
161/udp open  snmp
MAC Address: 02:42:C0:AB:5F:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 1083.61 seconds
```

[[Hack2.0/NMAP#Port scanning|NMAP Port service enum]]

```
root@attackdefense:~# nmap -p 161 -A -sU 192.171.95.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-06 15:36 UTC
Nmap scan report for target-1 (192.171.95.3)
Host is up (0.000031s latency).

PORT    STATE SERVICE VERSION
161/udp open  snmp    SNMPv1 server; net-snmp SNMPv3 server (public)
| snmp-info: 
|   enterprise: net-snmp
|   engineIDFormat: unknown
|   engineIDData: a40dce7005daa66400000000
|   snmpEngineBoots: 1
|_  snmpEngineTime: 23m33s
| snmp-interfaces: 
|   lo
|     IP address: 127.0.0.1  Netmask: 255.0.0.0
|     Type: softwareLoopback  Speed: 10 Mbps
|     Traffic stats: 9.46 Kb sent, 9.46 Kb received
|   eth0
|     IP address: 192.171.95.3  Netmask: 255.255.255.0
|     MAC address: 02:42:c0:ab:5f:03 (Unknown)
|     Type: ethernetCsmacd  Speed: 4 Gbps
|_    Traffic stats: 3.68 Mb sent, 3.93 Mb received
| snmp-netstat: 
|   TCP  127.0.0.1:22         0.0.0.0:0
|   TCP  127.0.0.1:22         127.0.0.1:60766
|   TCP  127.0.0.1:1313       0.0.0.0:0
|   TCP  127.0.0.1:60766      127.0.0.1:22
|   TCP  127.0.0.11:43741     0.0.0.0:0
|   UDP  0.0.0.0:161          *:*
|   UDP  0.0.0.0:35037        *:*
|_  UDP  127.0.0.11:49452     *:*
| snmp-processes: 
|   1: 
|     Name: supervisord
|     Path: /usr/bin/python
|     Params: /usr/bin/supervisord -n
|   9: 
|     Name: snmpd
|     Path: snmpd
|     Params: -c /etc/snmp/snmpd.conf
|   23: 
|     Name: sshd
|     Path: /usr/sbin/sshd
|   26: 
|     Name: listener
|     Path: /listener
|   27: 
|     Name: ssh
|     Path: ssh
|     Params: -i /opt/david_key -o StrictHostKeyChecking=no david@127.0.0.1
|   28: 
|     Name: sshd
|     Path: sshd: david [priv]
|   37: 
|     Name: sshd
|     Path: sshd: david@notty
|   38: 
|     Name: bash
|_    Path: -bash
| snmp-sysdescr: Linux victim-1 5.4.0-153-generic #170-Ubuntu SMP Fri Jun 16 13:43:31 UTC 2023 x86_64
|_  System uptime: 23m33.33s (141333 timeticks)
| snmp-win32-software: 
|   adduser-3.113+nmu3ubuntu4; 0-01-01T00:00:00
|   apt-1.2.29ubuntu0.1; 0-01-01T00:00:00
|   autotools-dev-20150820.1; 0-01-01T00:00:00
|   base-files-9.4ubuntu4.8; 0-01-01T00:00:00
|   base-passwd-3.5.39; 0-01-01T00:00:00
|   bash-4.3-14ubuntu1.2; 0-01-01T00:00:00
|   binutils-2.26.1-1ubuntu1~16.04.8; 0-01-01T00:00:00
|   bsdutils-1:2.27.1-6ubuntu3.6; 0-01-01T00:00:00
|   build-essential-12.1ubuntu2; 0-01-01T00:00:00
|   bzip2-1.0.6-8ubuntu0.1; 0-01-01T00:00:00
|   ca-certificates-20170717~16.04.2; 0-01-01T00:00:00
|   coreutils-8.25-2ubuntu3~16.04; 0-01-01T00:00:00
|   cpp-4:5.3.1-1ubuntu1; 0-01-01T00:00:00
|   cpp-5-5.4.0-6ubuntu1~16.04.11; 0-01-01T00:00:00
|   dash-0.5.8-2.1ubuntu2; 0-01-01T00:00:00
|   debconf-1.5.58ubuntu1; 0-01-01T00:00:00
|   debianutils-4.7; 0-01-01T00:00:00
|   dh-python-2.20151103ubuntu1.1; 0-01-01T00:00:00
|   diffutils-1:3.3-3; 0-01-01T00:00:00
|   dpkg-1.18.4ubuntu1.5; 0-01-01T00:00:00
|   dpkg-dev-1.18.4ubuntu1.5; 0-01-01T00:00:00
|   e2fslibs-1.42.13-1ubuntu1; 0-01-01T00:00:00
|   e2fsprogs-1.42.13-1ubuntu1; 0-01-01T00:00:00
|   fakeroot-1.20.2-1ubuntu1; 0-01-01T00:00:00
|   file-1:5.25-2ubuntu1.2; 0-01-01T00:00:00
|   findutils-4.6.0+git+20160126-2; 0-01-01T00:00:00
|   g++-4:5.3.1-1ubuntu1; 0-01-01T00:00:00
|   g++-5-5.4.0-6ubuntu1~16.04.11; 0-01-01T00:00:00
|   gcc-4:5.3.1-1ubuntu1; 0-01-01T00:00:00
|   gcc-5-5.4.0-6ubuntu1~16.04.11; 0-01-01T00:00:00
|   gcc-5-base-5.4.0-6ubuntu1~16.04.11; 0-01-01T00:00:00
|   gcc-6-base-6.0.1-0ubuntu1; 0-01-01T00:00:00
|   gnupg-1.4.20-1ubuntu3.3; 0-01-01T00:00:00
|   gpgv-1.4.20-1ubuntu3.3; 0-01-01T00:00:00
|   grep-2.25-1~16.04.1; 0-01-01T00:00:00
|   gzip-1.6-4ubuntu1; 0-01-01T00:00:00
|   hostname-3.16ubuntu2; 0-01-01T00:00:00
|   ifupdown-0.8.10ubuntu1.4; 0-01-01T00:00:00
|   init-1.29ubuntu4; 0-01-01T00:00:00
|   init-system-helpers-1.29ubuntu4; 0-01-01T00:00:00
|   initscripts-2.88dsf-59.3ubuntu2; 0-01-01T00:00:00
|   insserv-1.14.0-5ubuntu3; 0-01-01T00:00:00
|   iproute2-4.3.0-1ubuntu3.16.04.5; 0-01-01T00:00:00
|   isc-dhcp-client-4.3.3-5ubuntu12.10; 0-01-01T00:00:00
|   isc-dhcp-common-4.3.3-5ubuntu12.10; 0-01-01T00:00:00
|   krb5-locales-1.13.2+dfsg-5ubuntu2.1; 0-01-01T00:00:00
|   libacl1-2.2.52-3; 0-01-01T00:00:00
|   libalgorithm-diff-perl-1.19.03-1; 0-01-01T00:00:00
|   libalgorithm-diff-xs-perl-0.04-4build1; 0-01-01T00:00:00
|   libalgorithm-merge-perl-0.08-3; 0-01-01T00:00:00
|   libapparmor1-2.10.95-0ubuntu2.10; 0-01-01T00:00:00
|   libapt-pkg5.0-1.2.29ubuntu0.1; 0-01-01T00:00:00
|   libasan2-5.4.0-6ubuntu1~16.04.11; 0-01-01T00:00:00
|   libatm1-1:2.5.1-1.5; 0-01-01T00:00:00
|   libatomic1-5.4.0-6ubuntu1~16.04.11; 0-01-01T00:00:00
|   libattr1-1:2.4.47-2; 0-01-01T00:00:00
|   libaudit-common-1:2.4.5-1ubuntu2.1; 0-01-01T00:00:00
|   libaudit1-1:2.4.5-1ubuntu2.1; 0-01-01T00:00:00
|   libblkid1-2.27.1-6ubuntu3.6; 0-01-01T00:00:00
|   libbsd0-0.8.2-1; 0-01-01T00:00:00
|   libbz2-1.0-1.0.6-8ubuntu0.1; 0-01-01T00:00:00
|   libc-bin-2.23-0ubuntu11; 0-01-01T00:00:00
|   libc-dev-bin-2.23-0ubuntu11; 0-01-01T00:00:00
|   libc6-2.23-0ubuntu11; 0-01-01T00:00:00
|   libc6-dev-2.23-0ubuntu11; 0-01-01T00:00:00
|   libcap2-1:2.24-12; 0-01-01T00:00:00
|   libcap2-bin-1:2.24-12; 0-01-01T00:00:00
|   libcc1-0-5.4.0-6ubuntu1~16.04.11; 0-01-01T00:00:00
|   libcilkrts5-5.4.0-6ubuntu1~16.04.11; 0-01-01T00:00:00
|   libcomerr2-1.42.13-1ubuntu1; 0-01-01T00:00:00
|   libcryptsetup4-2:1.6.6-5ubuntu2.1; 0-01-01T00:00:00
|   libdb5.3-5.3.28-11ubuntu0.1; 0-01-01T00:00:00
|   libdebconfclient0-0.198ubuntu1; 0-01-01T00:00:00
|   libdevmapper1.02.1-2:1.02.110-1ubuntu10; 0-01-01T00:00:00
|   libdns-export162-1:9.10.3.dfsg.P4-8ubuntu1.14; 0-01-01T00:00:00
|   libdpkg-perl-1.18.4ubuntu1.5; 0-01-01T00:00:00
|   libedit2-3.1-20150325-1ubuntu2; 0-01-01T00:00:00
|   libexpat1-2.1.0-7ubuntu0.16.04.4; 0-01-01T00:00:00
|   libfakeroot-1.20.2-1ubuntu1; 0-01-01T00:00:00
|   libfdisk1-2.27.1-6ubuntu3.6; 0-01-01T00:00:00
|   libffi6-3.2.1-4; 0-01-01T00:00:00
|   libfile-fcntllock-perl-0.22-3; 0-01-01T00:00:00
|   libgcc-5-dev-5.4.0-6ubuntu1~16.04.11; 0-01-01T00:00:00
|   libgcc1-1:6.0.1-0ubuntu1; 0-01-01T00:00:00
|   libgcrypt20-1.6.5-2ubuntu0.5; 0-01-01T00:00:00
|   libgdbm3-1.8.3-13.1; 0-01-01T00:00:00
|   libgmp10-2:6.1.0+dfsg-2; 0-01-01T00:00:00
|   libgomp1-5.4.0-6ubuntu1~16.04.11; 0-01-01T00:00:00
|   libgpg-error0-1.21-2ubuntu1; 0-01-01T00:00:00
|   libgpm2-1.20.4-6.1; 0-01-01T00:00:00
|   libgssapi-krb5-2-1.13.2+dfsg-5ubuntu2.1; 0-01-01T00:00:00
|   libidn11-1.32-3ubuntu1.2; 0-01-01T00:00:00
|   libisc-export160-1:9.10.3.dfsg.P4-8ubuntu1.14; 0-01-01T00:00:00
|   libisl15-0.16.1-1; 0-01-01T00:00:00
|   libitm1-5.4.0-6ubuntu1~16.04.11; 0-01-01T00:00:00
|   libk5crypto3-1.13.2+dfsg-5ubuntu2.1; 0-01-01T00:00:00
|   libkeyutils1-1.5.9-8ubuntu1; 0-01-01T00:00:00
|   libkmod2-22-1ubuntu5.2; 0-01-01T00:00:00
|   libkrb5-3-1.13.2+dfsg-5ubuntu2.1; 0-01-01T00:00:00
|   libkrb5support0-1.13.2+dfsg-5ubuntu2.1; 0-01-01T00:00:00
|   liblsan0-5.4.0-6ubuntu1~16.04.11; 0-01-01T00:00:00
|   libltdl-dev-2.4.6-0.1; 0-01-01T00:00:00
|   libltdl7-2.4.6-0.1; 0-01-01T00:00:00
|   liblz4-1-0.0~r131-2ubuntu2; 0-01-01T00:00:00
|   liblzma5-5.1.1alpha+20120614-2ubuntu2; 0-01-01T00:00:00
|   libmagic1-1:5.25-2ubuntu1.2; 0-01-01T00:00:00
|   libmnl0-1.0.3-5; 0-01-01T00:00:00
|   libmount1-2.27.1-6ubuntu3.6; 0-01-01T00:00:00
|   libmpc3-1.0.3-1; 0-01-01T00:00:00
|   libmpdec2-2.4.2-1; 0-01-01T00:00:00
|   libmpfr4-3.1.4-1; 0-01-01T00:00:00
|   libmpx0-5.4.0-6ubuntu1~16.04.11; 0-01-01T00:00:00
|   libncurses5-6.0+20160213-1ubuntu1; 0-01-01T00:00:00
|   libncursesw5-6.0+20160213-1ubuntu1; 0-01-01T00:00:00
|   libpam-modules-1.1.8-3.2ubuntu2.1; 0-01-01T00:00:00
|   libpam-modules-bin-1.1.8-3.2ubuntu2.1; 0-01-01T00:00:00
|   libpam-runtime-1.1.8-3.2ubuntu2.1; 0-01-01T00:00:00
|   libpam0g-1.1.8-3.2ubuntu2.1; 0-01-01T00:00:00
|   libpcre3-2:8.38-3.1; 0-01-01T00:00:00
|   libperl-dev-5.22.1-9ubuntu0.6; 0-01-01T00:00:00
|   libperl5.22-5.22.1-9ubuntu0.6; 0-01-01T00:00:00
|   libprocps4-2:3.3.10-4ubuntu2.4; 0-01-01T00:00:00
|   libpython-stdlib-2.7.12-1~16.04; 0-01-01T00:00:00
|   libpython2.7-minimal-2.7.12-1ubuntu0~16.04.4; 0-01-01T00:00:00
|   libpython2.7-stdlib-2.7.12-1ubuntu0~16.04.4; 0-01-01T00:00:00
|   libpython3-stdlib-3.5.1-3; 0-01-01T00:00:00
|   libpython3.5-3.5.2-2ubuntu0~16.04.5; 0-01-01T00:00:00
|   libpython3.5-minimal-3.5.2-2ubuntu0~16.04.5; 0-01-01T00:00:00
|   libpython3.5-stdlib-3.5.2-2ubuntu0~16.04.5; 0-01-01T00:00:00
|   libquadmath0-5.4.0-6ubuntu1~16.04.11; 0-01-01T00:00:00
|   libreadline6-6.3-8ubuntu2; 0-01-01T00:00:00
|   libseccomp2-2.3.1-2.1ubuntu2~16.04.1; 0-01-01T00:00:00
|   libselinux1-2.4-3build2; 0-01-01T00:00:00
|   libsemanage-common-2.3-1build3; 0-01-01T00:00:00
|   libsemanage1-2.3-1build3; 0-01-01T00:00:00
|   libsepol1-2.4-2; 0-01-01T00:00:00
|   libsmartcols1-2.27.1-6ubuntu3.6; 0-01-01T00:00:00
|   libsqlite3-0-3.11.0-1ubuntu1.2; 0-01-01T00:00:00
|   libss2-1.42.13-1ubuntu1; 0-01-01T00:00:00
|   libssl1.0.0-1.0.2g-1ubuntu4.15; 0-01-01T00:00:00
|   libstdc++-5-dev-5.4.0-6ubuntu1~16.04.11; 0-01-01T00:00:00
|   libstdc++6-5.4.0-6ubuntu1~16.04.11; 0-01-01T00:00:00
|   libsystemd0-229-4ubuntu21.16; 0-01-01T00:00:00
|   libtinfo5-6.0+20160213-1ubuntu1; 0-01-01T00:00:00
|   libtool-2.4.6-0.1; 0-01-01T00:00:00
|   libtsan0-5.4.0-6ubuntu1~16.04.11; 0-01-01T00:00:00
|   libubsan0-5.4.0-6ubuntu1~16.04.11; 0-01-01T00:00:00
|   libudev1-229-4ubuntu21.16; 0-01-01T00:00:00
|   libusb-0.1-4-2:0.1.12-28; 0-01-01T00:00:00
|   libustr-1.0-1-1.0.4-5; 0-01-01T00:00:00
|   libuuid1-2.27.1-6ubuntu3.6; 0-01-01T00:00:00
|   libwrap0-7.6.q-25; 0-01-01T00:00:00
|   libx11-6-2:1.6.3-1ubuntu2.1; 0-01-01T00:00:00
|   libx11-data-2:1.6.3-1ubuntu2.1; 0-01-01T00:00:00
|   libxau6-1:1.0.8-1; 0-01-01T00:00:00
|   libxcb1-1.11.1-1ubuntu1; 0-01-01T00:00:00
|   libxdmcp6-1:1.1.2-1.1; 0-01-01T00:00:00
|   libxext6-2:1.3.3-1; 0-01-01T00:00:00
|   libxmuu1-2:1.1.2-2; 0-01-01T00:00:00
|   libxtables11-1.6.0-2ubuntu3; 0-01-01T00:00:00
|   linux-libc-dev-4.4.0-154.181; 0-01-01T00:00:00
|   login-1:4.2-3.1ubuntu5.3; 0-01-01T00:00:00
|   lsb-base-9.20160110ubuntu0.2; 0-01-01T00:00:00
|   make-4.1-6; 0-01-01T00:00:00
|   makedev-2.3.1-93ubuntu2~ubuntu16.04.1; 0-01-01T00:00:00
|   manpages-4.04-2; 0-01-01T00:00:00
|   manpages-dev-4.04-2; 0-01-01T00:00:00
|   mawk-1.3.3-17ubuntu2; 0-01-01T00:00:00
|   mime-support-3.59ubuntu1; 0-01-01T00:00:00
|   mount-2.27.1-6ubuntu3.6; 0-01-01T00:00:00
|   multiarch-support-2.23-0ubuntu11; 0-01-01T00:00:00
|   ncurses-base-6.0+20160213-1ubuntu1; 0-01-01T00:00:00
|   ncurses-bin-6.0+20160213-1ubuntu1; 0-01-01T00:00:00
|   ncurses-term-6.0+20160213-1ubuntu1; 0-01-01T00:00:00
|   net-tools-1.60-26ubuntu1; 0-01-01T00:00:00
|   netbase-5.3; 0-01-01T00:00:00
|   netcat-1.10-41; 0-01-01T00:00:00
|   netcat-traditional-1.10-41; 0-01-01T00:00:00
|   openssh-client-1:7.2p2-4ubuntu2.8; 0-01-01T00:00:00
|   openssh-server-1:7.2p2-4ubuntu2.8; 0-01-01T00:00:00
|   openssh-sftp-server-1:7.2p2-4ubuntu2.8; 0-01-01T00:00:00
|   openssl-1.0.2g-1ubuntu4.15; 0-01-01T00:00:00
|   passwd-1:4.2-3.1ubuntu5.3; 0-01-01T00:00:00
|   patch-2.7.5-1ubuntu0.16.04.1; 0-01-01T00:00:00
|   perl-5.22.1-9ubuntu0.6; 0-01-01T00:00:00
|   perl-base-5.22.1-9ubuntu0.6; 0-01-01T00:00:00
|   perl-modules-5.22-5.22.1-9ubuntu0.6; 0-01-01T00:00:00
|   procps-2:3.3.10-4ubuntu2.4; 0-01-01T00:00:00
|   python-2.7.12-1~16.04; 0-01-01T00:00:00
|   python-meld3-1.0.2-2; 0-01-01T00:00:00
|   python-minimal-2.7.12-1~16.04; 0-01-01T00:00:00
|   python-pkg-resources-20.7.0-1; 0-01-01T00:00:00
|   python2.7-2.7.12-1ubuntu0~16.04.4; 0-01-01T00:00:00
|   python2.7-minimal-2.7.12-1ubuntu0~16.04.4; 0-01-01T00:00:00
|   python3-3.5.1-3; 0-01-01T00:00:00
|   python3-chardet-2.3.0-2; 0-01-01T00:00:00
|   python3-minimal-3.5.1-3; 0-01-01T00:00:00
|   python3-pkg-resources-20.7.0-1; 0-01-01T00:00:00
|   python3-requests-2.9.1-3ubuntu0.1; 0-01-01T00:00:00
|   python3-six-1.10.0-3; 0-01-01T00:00:00
|   python3-urllib3-1.13.1-2ubuntu0.16.04.3; 0-01-01T00:00:00
|   python3.5-3.5.2-2ubuntu0~16.04.5; 0-01-01T00:00:00
|   python3.5-minimal-3.5.2-2ubuntu0~16.04.5; 0-01-01T00:00:00
|   readline-common-6.3-8ubuntu2; 0-01-01T00:00:00
|   rename-0.20-4; 0-01-01T00:00:00
|   sed-4.2.2-7; 0-01-01T00:00:00
|   sensible-utils-0.0.9ubuntu0.16.04.1; 0-01-01T00:00:00
|   ssh-import-id-5.5-0ubuntu1; 0-01-01T00:00:00
|   supervisor-3.2.0-2ubuntu0.2; 0-01-01T00:00:00
|   systemd-229-4ubuntu21.16; 0-01-01T00:00:00
|   systemd-sysv-229-4ubuntu21.16; 0-01-01T00:00:00
|   sysv-rc-2.88dsf-59.3ubuntu2; 0-01-01T00:00:00
|   sysvinit-utils-2.88dsf-59.3ubuntu2; 0-01-01T00:00:00
|   tar-1.28-2.1ubuntu0.1; 0-01-01T00:00:00
|   tcpd-7.6.q-25; 0-01-01T00:00:00
|   ubuntu-keyring-2012.05.19; 0-01-01T00:00:00
|   util-linux-2.27.1-6ubuntu3.6; 0-01-01T00:00:00
|   vim-2:7.4.1689-3ubuntu1.3; 0-01-01T00:00:00
|   vim-common-2:7.4.1689-3ubuntu1.3; 0-01-01T00:00:00
|   vim-runtime-2:7.4.1689-3ubuntu1.3; 0-01-01T00:00:00
|   wget-1.17.1-1ubuntu1.5; 0-01-01T00:00:00
|   xauth-1:1.0.9-1ubuntu2; 0-01-01T00:00:00
|   xz-utils-5.1.1alpha+20120614-2ubuntu2; 0-01-01T00:00:00
|_  zlib1g-1:1.2.8.dfsg-2ubuntu4.1; 0-01-01T00:00:00
MAC Address: 02:42:C0:AB:5F:03 (Unknown)
Too many fingerprints match this host to give specific OS details
Network Distance: 1 hop
Service Info: Host: victim-1

TRACEROUTE
HOP RTT     ADDRESS
1   0.03 ms target-1 (192.171.95.3)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 2.61 seconds
```
