`ris:Questionnaire`  The objectives of this lab are to:

1. Identify the port running a Bind DNS server.
2. Identify the port running a TFTP server.
3. Identify the port running the SNMP server.

`ris:Tools` [[Hack2.0/NMAP|NMAP]]

[[ip]]

```
root@INE:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
1269: eth0@if1270: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:09 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.9/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
1272: eth1@if1273: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:cf:ea:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.207.234.2/24 brd 192.207.234.255 scope global eth1
       valid_lft forever preferred_lft forever
```

[[ping]]

```
root@INE:~# ping -c 5 192.207.234.3
PING 192.207.234.3 (192.207.234.3) 56(84) bytes of data.
64 bytes from 192.207.234.3: icmp_seq=1 ttl=64 time=0.093 ms
64 bytes from 192.207.234.3: icmp_seq=2 ttl=64 time=0.045 ms
64 bytes from 192.207.234.3: icmp_seq=3 ttl=64 time=0.048 ms
64 bytes from 192.207.234.3: icmp_seq=4 ttl=64 time=0.045 ms
64 bytes from 192.207.234.3: icmp_seq=5 ttl=64 time=0.045 ms

--- 192.207.234.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4095ms
rtt min/avg/max/mdev = 0.045/0.055/0.093/0.018 ms
```

[[Hack2.0/NMAP#General Scan|NMAP General Scan]]

```
root@INE:~# nmap 192.207.234.3
Starting Nmap 7.92 ( https://nmap.org ) at 2023-07-06 15:59 IST
Nmap scan report for agfgtv700qsn9advfkki3801l.temp-network_a-207-234 (192.207.234.3)
Host is up (0.000010s latency).
All 1000 scanned ports on agfgtv700qsn9advfkki3801l.temp-network_a-207-234 (192.207.234.3) are in ignored states.
Not shown: 1000 closed tcp ports (reset)
MAC Address: 02:42:C0:CF:EA:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.24 seconds
```

[[Hack2.0/NMAP#scan all ports|NMAP All port scan]]

```
root@INE:~# nmap -p- 192.207.234.3
Starting Nmap 7.92 ( https://nmap.org ) at 2023-07-06 15:59 IST                                                                                                                             
Nmap scan report for agfgtv700qsn9advfkki3801l.temp-network_a-207-234 (192.207.234.3)                                                                                                       
Host is up (0.0000090s latency).                                                                                                                                                            
Not shown: 65534 closed tcp ports (reset)                                                                                                                                                   
PORT    STATE SERVICE                                                                                                                                                                       
177/tcp open  xdmcp                                                                                                                                                                         
MAC Address: 02:42:C0:CF:EA:03 (Unknown)                                                                                                                                                    
                                                                                                                                                                                            
Nmap done: 1 IP address (1 host up) scanned in 1.15 seconds
```

[[Hack2.0/NMAP#UDP Scan|NMAP UDP scan]]

```
root@INE:~# nmap -sU -p 1-250 192.207.234.3
Starting Nmap 7.92 ( https://nmap.org ) at 2023-07-06 16:10 IST
Nmap scan report for agfgtv700qsn9advfkki3801l.temp-network_a-207-234 (192.207.234.3)
Host is up (0.000044s latency).
Not shown: 247 closed udp ports (port-unreach)
PORT    STATE         SERVICE
134/udp open|filtered ingres-net
177/udp open|filtered xdmcp
234/udp open|filtered unknown
MAC Address: 02:42:C0:CF:EA:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 280.03 seconds
```

Now that we got the ports that are running services let us try to find the service versions

[[Hack2.0/NMAP#Service version|NMAP TCP Service enumeration]]

```
root@INE:~# nmap -p 177 -sV -A 192.207.234.3
Starting Nmap 7.92 ( https://nmap.org ) at 2023-07-06 16:16 IST
Nmap scan report for agfgtv700qsn9advfkki3801l.temp-network_a-207-234 (192.207.234.3)
Host is up (0.000037s latency).

PORT    STATE SERVICE VERSION
177/tcp open  domain  ISC BIND 9.10.3-P4 (Ubuntu Linux)
| dns-nsid: 
|_  bind.version: 9.10.3-P4-Ubuntu
MAC Address: 02:42:C0:CF:EA:03 (Unknown)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.6
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   0.04 ms agfgtv700qsn9advfkki3801l.temp-network_a-207-234 (192.207.234.3)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 41.10 seconds
```

[[Hack2.0/NMAP#Service version|NMAP UDP service version]]

```
root@INE:~# nmap -sU -p 134,177,234 -sV 192.207.234.3
Starting Nmap 7.92 ( https://nmap.org ) at 2023-07-06 16:19 IST
Nmap scan report for agfgtv700qsn9advfkki3801l.temp-network_a-207-234 (192.207.234.3)
Host is up (0.000058s latency).

PORT    STATE         SERVICE    VERSION
134/udp open|filtered ingres-net
177/udp open          domain     ISC BIND 9.10.3-P4 (Ubuntu Linux)
234/udp open          snmp       SNMPv1 server; net-snmp SNMPv3 server (public)
MAC Address: 02:42:C0:CF:EA:03 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 100.40 seconds
```

[[Hack2.0/NMAP#Specific script category|NMAP discovery script]]

```
root@INE:~# nmap -sU -p 134,177,234 -sV 192.207.234.3 --script=discovery
Starting Nmap 7.92 ( https://nmap.org ) at 2023-07-06 16:23 IST
Pre-scan script results:
| targets-asn: 
|_  targets-asn.asn is a mandatory parameter
|_hostmap-robtex: *TEMPORARILY DISABLED* due to changes in Robtex's API. See https://www.robtex.com/api/
|_http-robtex-shared-ns: *TEMPORARILY DISABLED* due to changes in Robtex's API. See https://www.robtex.com/api/
| broadcast-igmp-discovery: 
|   10.1.0.1
|     Interface: eth0
|     Version: 2
|     Group: 224.0.0.106
|     Description: All-Snoopers (rfc4286)
|   192.207.234.1
|     Interface: eth1
|     Version: 2
|     Group: 224.0.0.106
|     Description: All-Snoopers (rfc4286)
|_  Use the newtargets script-arg to add the results as targets
Nmap scan report for agfgtv700qsn9advfkki3801l.temp-network_a-207-234 (192.207.234.3)
Host is up (0.000034s latency).

PORT    STATE         SERVICE    VERSION
134/udp open|filtered ingres-net
177/udp open          domain     ISC BIND 9.10.3-P4 (Ubuntu Linux)
|_dns-cache-snoop: 0 of 100 tested domains are cached.
| dns-nsec3-enum: 
|_  DNSSEC NSEC3 not supported
| dns-nsid: 
|_  bind.version: 9.10.3-P4-Ubuntu
| dns-nsec-enum: 
|_  No NSEC records found
234/udp open          snmp       SNMPv1 server; net-snmp SNMPv3 server (public)
| snmp-info: 
|   enterprise: net-snmp
|   engineIDFormat: unknown
|   engineIDData: 88a97d1df096a66400000000
|   snmpEngineBoots: 1
|_  snmpEngineTime: 28m03s
MAC Address: 02:42:C0:CF:EA:03 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_whois-domain: You should provide a domain name.
|_whois-ip: ERROR: Script execution failed (use -d to debug)
|_fcrdns: PASS (agfgtv700qsn9advfkki3801l.temp-network_a-207-234)
|_asn-query: No Answers
Bug in ip-geolocation-geoplugin: no string output.
|_sniffer-detect: Likely in promiscuous mode (tests: "11111111")
| dns-brute: 
|_  DNS Brute-force hostnames: No results.

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 139.33 seconds
```

[[tftp#Basic tftp connect command|Connect with tftp]]

```
root@INE:~# tftp 192.207.234.3 134
tftp> status
Connected to 192.207.234.3.
Mode: netascii Verbose: off Tracing: off
Rexmt-interval: 5 seconds, Max-timeout: 25 seconds
tftp> 
```

