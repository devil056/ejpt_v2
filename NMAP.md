#### nmap (1)             - Network exploration tool and security / port scanner

`ris:Links`  [Reference for NMAP](https://nmap.org/book/toc.html)

This is the basic explanation that you do find when you search for the tool. I think it is a best friend for every pentester who is always ready to help you out. This notes is specifically for the different scans we can perform using the nmap.

NMAP scripts are located in **/usr/share/nmap/scripts** and the other way to find the available scripts is with the help of script.db file even though it is mentioned as db file it is a txt file specially formatted.

- NMAP is a free and open source network scanner that can be used to discover hosts on a network as well as scan targets for open ports
- It can also be used to enumerate the services running on open ports as well as the operating system running on the target system.
- We can output the results of our NMAP scan in to a format that can be imported into MSF for vulnerability detection and exploitation.


#### Script find

```
grep "ftp" /usr/share/nmap/scripts/script.db
```

#### Script help

```
nmap --script-help <script-name>
```

```
└─$ nmap                          
Nmap 7.94 ( https://nmap.org )
Usage: nmap [Scan Type(s)] [Options] {target specification}
TARGET SPECIFICATION:
  Can pass hostnames, IP addresses, networks, etc.
  Ex: scanme.nmap.org, microsoft.com/24, 192.168.0.1; 10.0.0-255.1-254
  -iL <inputfilename>: Input from list of hosts/networks
  -iR <num hosts>: Choose random targets
  --exclude <host1[,host2][,host3],...>: Exclude hosts/networks
  --excludefile <exclude_file>: Exclude list from file
HOST DISCOVERY:
  -sL: List Scan - simply list targets to scan
  -sn: Ping Scan - disable port scan
  -Pn: Treat all hosts as online -- skip host discovery
  -PS/PA/PU/PY[portlist]: TCP SYN/ACK, UDP or SCTP discovery to given ports
  -PE/PP/PM: ICMP echo, timestamp, and netmask request discovery probes
  -PO[protocol list]: IP Protocol Ping
  -n/-R: Never do DNS resolution/Always resolve [default: sometimes]
  --dns-servers <serv1[,serv2],...>: Specify custom DNS servers
  --system-dns: Use OS's DNS resolver
  --traceroute: Trace hop path to each host
SCAN TECHNIQUES:
  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
  -sU: UDP Scan
  -sN/sF/sX: TCP Null, FIN, and Xmas scans
  --scanflags <flags>: Customize TCP scan flags
  -sI <zombie host[:probeport]>: Idle scan
  -sY/sZ: SCTP INIT/COOKIE-ECHO scans
  -sO: IP protocol scan
  -b <FTP relay host>: FTP bounce scan
PORT SPECIFICATION AND SCAN ORDER:
  -p <port ranges>: Only scan specified ports
    Ex: -p22; -p1-65535; -p U:53,111,137,T:21-25,80,139,8080,S:9
  --exclude-ports <port ranges>: Exclude the specified ports from scanning
  -F: Fast mode - Scan fewer ports than the default scan
  -r: Scan ports sequentially - don't randomize
  --top-ports <number>: Scan <number> most common ports
  --port-ratio <ratio>: Scan ports more common than <ratio>
SERVICE/VERSION DETECTION:
  -sV: Probe open ports to determine service/version info
  --version-intensity <level>: Set from 0 (light) to 9 (try all probes)
  --version-light: Limit to most likely probes (intensity 2)
  --version-all: Try every single probe (intensity 9)
  --version-trace: Show detailed version scan activity (for debugging)
SCRIPT SCAN:
  -sC: equivalent to --script=default
  --script=<Lua scripts>: <Lua scripts> is a comma separated list of
           directories, script-files or script-categories
  --script-args=<n1=v1,[n2=v2,...]>: provide arguments to scripts
  --script-args-file=filename: provide NSE script args in a file
  --script-trace: Show all data sent and received
  --script-updatedb: Update the script database.
  --script-help=<Lua scripts>: Show help about scripts.
           <Lua scripts> is a comma-separated list of script-files or
           script-categories.
OS DETECTION:
  -O: Enable OS detection
  --osscan-limit: Limit OS detection to promising targets
  --osscan-guess: Guess OS more aggressively
TIMING AND PERFORMANCE:
  Options which take <time> are in seconds, or append 'ms' (milliseconds),
  's' (seconds), 'm' (minutes), or 'h' (hours) to the value (e.g. 30m).
  -T<0-5>: Set timing template (higher is faster)
  --min-hostgroup/max-hostgroup <size>: Parallel host scan group sizes
  --min-parallelism/max-parallelism <numprobes>: Probe parallelization
  --min-rtt-timeout/max-rtt-timeout/initial-rtt-timeout <time>: Specifies
      probe round trip time.
  --max-retries <tries>: Caps number of port scan probe retransmissions.
  --host-timeout <time>: Give up on target after this long
  --scan-delay/--max-scan-delay <time>: Adjust delay between probes
  --min-rate <number>: Send packets no slower than <number> per second
  --max-rate <number>: Send packets no faster than <number> per second
FIREWALL/IDS EVASION AND SPOOFING:
  -f; --mtu <val>: fragment packets (optionally w/given MTU)
  -D <decoy1,decoy2[,ME],...>: Cloak a scan with decoys
  -S <IP_Address>: Spoof source address
  -e <iface>: Use specified interface
  -g/--source-port <portnum>: Use given port number
  --proxies <url1,[url2],...>: Relay connections through HTTP/SOCKS4 proxies
  --data <hex string>: Append a custom payload to sent packets
  --data-string <string>: Append a custom ASCII string to sent packets
  --data-length <num>: Append random data to sent packets
  --ip-options <options>: Send packets with specified ip options
  --ttl <val>: Set IP time-to-live field
  --spoof-mac <mac address/prefix/vendor name>: Spoof your MAC address
  --badsum: Send packets with a bogus TCP/UDP/SCTP checksum
OUTPUT:
  -oN/-oX/-oS/-oG <file>: Output scan in normal, XML, s|<rIpt kIddi3,
     and Grepable format, respectively, to the given filename.
  -oA <basename>: Output in the three major formats at once
  -v: Increase verbosity level (use -vv or more for greater effect)
  -d: Increase debugging level (use -dd or more for greater effect)
  --reason: Display the reason a port is in a particular state
  --open: Only show open (or possibly open) ports
  --packet-trace: Show all packets sent and received
  --iflist: Print host interfaces and routes (for debugging)
  --append-output: Append to rather than clobber specified output files
  --resume <filename>: Resume an aborted scan
  --noninteractive: Disable runtime interactions via keyboard
  --stylesheet <path/URL>: XSL stylesheet to transform XML output to HTML
  --webxml: Reference stylesheet from Nmap.Org for more portable XML
  --no-stylesheet: Prevent associating of XSL stylesheet w/XML output
MISC:
  -6: Enable IPv6 scanning
  -A: Enable OS detection, version detection, script scanning, and traceroute
  --datadir <dirname>: Specify custom Nmap data file location
  --send-eth/--send-ip: Send using raw ethernet frames or IP packets
  --privileged: Assume that the user is fully privileged
  --unprivileged: Assume the user lacks raw socket privileges
  -V: Print version number
  -h: Print this help summary page.
EXAMPLES:
  nmap -v -A scanme.nmap.org
  nmap -v -sn 192.168.0.0/16 10.0.0.0/8
  nmap -v -iR 10000 -Pn -p 80
SEE THE MAN PAGE (https://nmap.org/book/man.html) FOR MORE OPTIONS AND EXAMPLES

```

##### Ping scan -sn or Host Discovery
This ping scan is used to find the IP addresses of the devices that are currently active on the target network

```
└─$ nmap -sn 10.0.2.10/24         
Starting Nmap 7.94 ( https://nmap.org ) at 2023-07-02 01:02 IST
Nmap scan report for 10.0.2.1
Host is up (0.00028s latency).
Nmap scan report for 10.0.2.10
Host is up (0.00016s latency).
Nmap done: 256 IP addresses (2 hosts up) scanned in 3.13 seconds
```

You can also refer [[netdiscover]] 

#### General Scan

```
nmap <target-ip>
```

##### Port scanning

There are multiple ways to perform a port scanning. We are given an option to scan initial 1000 ports by default even those 1000 are some of the well known ports of common services that are usually used.

with ranges:

```
nmap -p 100-1000 <target-ip>
```

top ports is by default covered in the nmap help part

The same goes for the specific ports as well.

#### scan all ports:

```
nmap -p- <target-ip>
```

#### Service version
If we want to find the service versions just use the -sV option:

```
nmap -p 80 -sV <target-ip>
```

Incase if you are worried about getting caught while performing the nmap scan we can use the Timing option

```
nmap -p- -T3 <target-ip>
```

Also there are two types of scans one is TCP while the other is UDP scan. To perform the UDP scan we do use the -sU option. UDP scans are too slow so in order to speed up the process you can use the timing option.

#### OS detection

```
nmap -p 80 -sV -O <target-ip>
```

#### UDP Scan

Generally the UDP scans are so slow we might need to the flag T and set our preferred speed to get proper results.

```
nmap -sU <target-ip>
```

#### Script scan

some of the default scripts include the discovery of the known services and vulnerabilities.

```
sudo nmap -iL ip_list -sV -O -sC
```

#### Specific script category 
The different kind of categories that we have are in the [link](https://nmap.org/book/nse-usage.html)

```
nmap -p 1,2,3 <target-ip> --script=brute
```

#### Specific script

```
nmap -p <port no.> --script <script-name> <target-ip>
```

### Zenmap

It is a tool built upon the NMAP which mostly uses the GUI. Apart from that all the rest of the feaures are similar to that of the NMAP.

#### Ping scan or host scan:
you need to specify the target network range and the type of scan you wish to perform so that we can identify the devices on the network. The command that is generated and the output we get as response are tagged for reference below please check.

```
nmap -sn 10.0.2.0/24
```

```
Starting Nmap 7.92 ( https://nmap.org ) at 2023-07-06 06:19 UTC
Nmap scan report for 10.0.2.1
Host is up (0.00020s latency).
MAC Address: 52:54:00:12:35:00 (QEMU virtual NIC)
Nmap scan report for 10.0.2.2
Host is up (0.00017s latency).
MAC Address: 52:54:00:12:35:00 (QEMU virtual NIC)
Nmap scan report for 10.0.2.3
Host is up (0.00012s latency).
MAC Address: 08:00:27:F8:A6:EB (Oracle VirtualBox virtual NIC)
Nmap scan report for 10.0.2.5
Host is up.
Nmap done: 256 IP addresses (4 hosts up) scanned in 2.06 seconds
```

We also get the topology and the host details tabs along with the scan which gives the information as below.

![[Pasted image 20230706115642.png]] ![[Pasted image 20230706115748.png]]

#### Using the IP file to perform the Stealthy scan

```
nmap -iL ipcs -sV -O
```

```
└─$ sudo nmap -iL ip_list -sV -O
[sudo] password for kali: 
Starting Nmap 7.94 ( https://nmap.org ) at 2023-07-06 02:47 EDT
Stats: 0:00:30 elapsed; 0 hosts completed (3 up), 3 undergoing Script Scan
NSE Timing: About 0.00% done
Stats: 0:00:30 elapsed; 0 hosts completed (3 up), 3 undergoing Script Scan
NSE Timing: About 83.33% done; ETC: 02:48 (0:00:00 remaining)
Nmap scan report for 10.0.2.1
Host is up (0.0050s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
53/tcp open  domain  dnsmasq 2.71
MAC Address: 52:54:00:12:35:00 (QEMU virtual NIC)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.94%E=4%D=7/6%OT=53%CT=1%CU=33812%PV=Y%DS=1%DC=D%G=Y%M=525400%TM
OS:=64A663A7%P=x86_64-pc-linux-gnu)SEQ(SP=11%GCD=1%ISR=50%TI=I%CI=I%II=RI%S
OS:S=O%TS=U)SEQ(SP=29%GCD=1%ISR=4E%TI=I%CI=I%II=RI%SS=O%TS=U)SEQ(SP=30%GCD=
OS:1%ISR=55%TI=I%CI=I%II=RI%TS=U)SEQ(SP=35%GCD=1%ISR=4F%TI=I%CI=I%II=RI%SS=
OS:O%TS=U)SEQ(SP=35%GCD=1%ISR=56%TI=I%CI=I%II=RI%SS=O%TS=U)OPS(O1=M5B4%O2=M
OS:5B4%O3=M5B4%O4=M5B4%O5=M5B4%O6=M5B4)WIN(W1=8000%W2=8000%W3=8000%W4=8000%
OS:W5=8000%W6=8000)ECN(R=Y%DF=N%T=FF%W=8000%O=M5B4%CC=N%Q=)T1(R=Y%DF=N%T=FF
OS:%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=Y%DF=N%T=FF%W=8000%S=O%A=S+%F=AS%O=M5
OS:B4%RD=0%Q=)T4(R=Y%DF=N%T=FF%W=8000%S=A%A=S%F=AR%O=%RD=0%Q=)T5(R=Y%DF=N%T
OS:=FF%W=8000%S=A%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=N%T=FF%W=8000%S=A%A=S%F=AR
OS:%O=%RD=0%Q=)T7(R=Y%DF=N%T=FF%W=8000%S=A%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N
OS:%T=FF%IPL=38%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=S%T=FF%CD
OS:=S)

Network Distance: 1 hop

Nmap scan report for 10.0.2.2
Host is up (0.00029s latency).
Not shown: 998 filtered tcp ports (no-response)
PORT    STATE SERVICE       VERSION
135/tcp open  msrpc         Microsoft Windows RPC
445/tcp open  microsoft-ds?
MAC Address: 52:54:00:12:35:00 (QEMU virtual NIC)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: VoIP phone|webcam|specialized|firewall|general purpose
Running (JUST GUESSING): Grandstream embedded (91%), Garmin embedded (90%), 2N embedded (88%), FireBrick embedded (85%), Cognex embedded (85%), lwIP (85%)
OS CPE: cpe:/h:grandstream:gxp1105 cpe:/h:garmin:virb_elite cpe:/h:2n:helios cpe:/h:firebrick:fb2700 cpe:/a:lwip_project:lwip
Aggressive OS guesses: Grandstream GXP1105 VoIP phone (91%), Garmin Virb Elite action camera (90%), 2N Helios IP VoIP doorbell (88%), FireBrick FB2700 firewall (85%), Cognex DataMan 200 ID reader (lwIP TCP/IP stack) (85%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Nmap scan report for 10.0.2.3
Host is up (0.000049s latency).
All 1000 scanned ports on 10.0.2.3 are in ignored states.
Not shown: 1000 filtered tcp ports (proto-unreach)
MAC Address: 08:00:27:F8:A6:EB (Oracle VirtualBox virtual NIC)
Too many fingerprints match this host to give specific OS details
Network Distance: 1 hop

Nmap scan report for 10.0.2.5
Host is up (0.000019s latency).
All 1000 scanned ports on 10.0.2.5 are in ignored states.
Not shown: 1000 closed tcp ports (reset)
Too many fingerprints match this host to give specific OS details
Network Distance: 0 hops

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 4 IP addresses (4 hosts up) scanned in 32.19 seconds
```