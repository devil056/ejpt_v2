#### netdiscover

This tool is also used to find the devices active on the network and it sends the arp request.

```
└─$ sudo netdiscover -h
Netdiscover 0.10 [Active/passive ARP reconnaissance tool]
Written by: Jaime Penalba <jpenalbae@gmail.com>

Usage: netdiscover [-i device] [-r range | -l file | -p] [-m file] [-F filter] [-s time] [-c count] [-n node] [-dfPLNS]
  -i device: your network device
  -r range: scan a given range instead of auto scan. 192.168.6.0/24,/16,/8
  -l file: scan the list of ranges contained into the given file
  -p passive mode: do not send anything, only sniff
  -m file: scan a list of known MACs and host names
  -F filter: customize pcap filter expression (default: "arp")
  -s time: time to sleep between each ARP request (milliseconds)
  -c count: number of times to send each ARP request (for nets with packet loss)
  -n node: last source IP octet used for scanning (from 2 to 253)
  -d ignore home config files for autoscan and fast mode
  -f enable fastmode scan, saves a lot of time, recommended for auto
  -P print results in a format suitable for parsing by another program and stop after active scan
  -L similar to -P but continue listening after the active scan is completed
  -N Do not print header. Only valid when -P or -L is enabled.
  -S enable sleep time suppression between each request (hardcore mode)

If -r, -l or -p are not enabled, netdiscover will scan for common LAN addresses.
```

##### Using the tool to find devices
```
sudo netdiscover -i eth0 -r <the range we got from ip command>
```

```
 Currently scanning: Finished!   |   Screen View: Unique Hosts                                                                                                                                                                            
                                                                                                                                                                                                                                          
 3 Captured ARP Req/Rep packets, from 3 hosts.   Total size: 180                                                                                                                                                                          
 _____________________________________________________________________________
   IP            At MAC Address     Count     Len  MAC Vendor / Hostname      
 -----------------------------------------------------------------------------
 10.0.2.1        52:54:00:12:35:00      1      60  Unknown vendor                                                                                                                                                                         
 10.0.2.2        52:54:00:12:35:00      1      60  Unknown vendor                                                                                                                                                                         
 10.0.2.3        08:00:27:87:3c:83      1      60  PCS Systemtechnik GmbH   
```