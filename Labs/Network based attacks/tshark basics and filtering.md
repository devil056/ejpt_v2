**HTTP** is probably the most common and widely used protocol on the Internet. Unfortunately, HTTP is plain text and an attacker having access to the network can sniff and read the data within the packets effortlessly. In this lab, you will analyze and extract interesting information from the HTTP traffic using tshark.

Start the lab, locate the PCAP file in the current directory and use tshark to answer the following questions:

Questions:  
Set A:1. Command to show only the HTTP traffic from a PCAP file?

1. Command to show only the IP packets sent from IP address 192.168.252.128 to IP address 52.32.74.91?
2. Command to print only packets containing GET requests?
3. Command to print only packets only source IP and URL for all GET request packets?
Set B:1. How many HTTP packets contain the "password" string?
1. What is the destination IP address for GET requests sent for New York Times (www.nytimes.com)?
2. What is the session ID being used by 192.168.252.128 for Amazon India store (amazon.in)?
3. What type of OS the machine on IP address 192.168.252.128 is using (i.e. Windows/Linux/MacOS/Solaris/Unix/BSD)? Bonus: Can you also guess the distribution/flavor?

---

filtering the file to see the HTTP traffic:

```
tshark -r HTTP_traffic.pcap -Y 'http'
```

command to print the packets between two ip addresses:

```
tshark -r HTTP_traffic.pcap -Y 'ip.src==192.168.252.128 && ip.dst==52.32.74.91'
```

filtering based on the http get method

```
 tshark -r HTTP_traffic.pcap -Y 'http.request.method==GET'
```

printing only certain fields

```
tshark -r HTTP_traffic.pcap -Y 'http.request.method==GET' -Tfields -e frame.time -e ip.src -e http.request.full_uri
```

checking for passwords

```
tshark -r HTTP_traffic.pcap -Y 'http contains password'
```

destination from the nytimes.com 

```
tshark -r HTTP_traffic.pcap -Y 'http.request.method==GET && http.host==www.nytimes.com' -Tfields -e ip.dst
```

session id:

```
tshark -r HTTP_traffic.pcap -Y 'ip contains amazon.in && ip.src==192.168.252.128' -Tfields -e ip.src -e http.cookie
```

os finding

```
tshark -r HTTP_traffic.pcap -Y 'ip.src==192.168.252.128 && http' -Tfields -e http.user_agent
```

