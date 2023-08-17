Your task is to discover the live host machines using the provided Zenmap tool. The subnet mask you need to focus on is "255.255.240.0" and CIDR 20.
Objective: Discover all available live hosts.
`ris:Tools` [[Hack2.0/NMAP#Zenmap|ZENMAP]]

Find your IP :

In the powershell : 
```
ipconfig /all
```

```
PS C:\Users\Administrator> ipconfig /all

Windows IP Configuration

   Host Name . . . . . . . . . . . . : AttackDefense
   Primary Dns Suffix  . . . . . . . :
   Node Type . . . . . . . . . . . . : Hybrid
   IP Routing Enabled. . . . . . . . : No
   WINS Proxy Enabled. . . . . . . . : No
   DNS Suffix Search List. . . . . . : ap-south-1.ec2-utilities.amazonaws.com
                                       ap-southeast-1.ec2-utilities.amazonaws.com
                                       ap-southeast-1.compute.internal
                                       ap-south-1.compute.internal

Ethernet adapter Ethernet:

   Connection-specific DNS Suffix  . : ap-south-1.compute.internal
   Description . . . . . . . . . . . : AWS PV Network Device #0
   Physical Address. . . . . . . . . : 02-74-55-E8-E1-34
   DHCP Enabled. . . . . . . . . . . : Yes
   Autoconfiguration Enabled . . . . : Yes
   Link-local IPv6 Address . . . . . : fe80::914e:e4af:6148:fee2%4(Preferred)
   IPv4 Address. . . . . . . . . . . : 10.5.18.27(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.240.0
   Lease Obtained. . . . . . . . . . : Thursday, July 6, 2023 7:14:33 AM
   Lease Expires . . . . . . . . . . : Thursday, July 6, 2023 8:14:33 AM
   Default Gateway . . . . . . . . . : 10.5.16.1
   DHCP Server . . . . . . . . . . . : 10.5.16.1
   DHCPv6 IAID . . . . . . . . . . . : 118418632
   DHCPv6 Client DUID. . . . . . . . : 00-01-00-01-2C-38-22-30-02-74-55-E8-E1-34
   DNS Servers . . . . . . . . . . . : 10.5.0.2
   NetBIOS over Tcpip. . . . . . . . : Enabled
PS C:\Users\Administrator>
```

Now we can launch the zenmap and run the ping scan to check the available devices on network. As it was mentioned it is of CIDR 20. The command if we are to use the nmap would be

```
nmap -sn 10.5.18.0/20
```

```
Starting Nmap 7.91 ( https://nmap.org ) at 2023-07-06 07:17 Coordinated Universal Time

Nmap scan report for ip-10-5-16-1.ap-south-1.compute.internal (10.5.16.1)

Host is up (0.00s latency).

MAC Address: 02:B5:F8:B3:8D:26 (Unknown)

Nmap scan report for ip-10-5-16-44.ap-south-1.compute.internal (10.5.16.44)

Host is up (0.00s latency).

MAC Address: 02:73:CE:2A:1B:90 (Unknown)

Nmap scan report for ip-10-5-19-77.ap-south-1.compute.internal (10.5.19.77)

Host is up (0.00s latency).

MAC Address: 02:ED:27:20:EC:14 (Unknown)

Nmap scan report for ip-10-5-21-148.ap-south-1.compute.internal (10.5.21.148)

Host is up.

MAC Address: 02:5E:4D:80:7A:6C (Unknown)

Nmap scan report for ip-10-5-23-125.ap-south-1.compute.internal (10.5.23.125)

Host is up (0.00s latency).

MAC Address: 02:CF:62:8A:77:CE (Unknown)

Nmap scan report for ip-10-5-23-217.ap-south-1.compute.internal (10.5.23.217)

Host is up (0.00s latency).

MAC Address: 02:D9:40:5E:4C:26 (Unknown)

Nmap scan report for ip-10-5-25-69.ap-south-1.compute.internal (10.5.25.69)

Host is up (0.00s latency).

MAC Address: 02:06:4D:80:CD:36 (Unknown)

Nmap scan report for ip-10-5-26-246.ap-south-1.compute.internal (10.5.26.246)

Host is up (0.00s latency).

MAC Address: 02:CF:F7:C0:97:FA (Unknown)

Nmap scan report for ip-10-5-28-11.ap-south-1.compute.internal (10.5.28.11)

Host is up (0.00s latency).

MAC Address: 02:15:E5:C8:52:08 (Unknown)

Nmap scan report for ip-10-5-28-45.ap-south-1.compute.internal (10.5.28.45)

Host is up (0.00s latency).

MAC Address: 02:DF:10:BD:81:F6 (Unknown)

Nmap scan report for ip-10-5-28-166.ap-south-1.compute.internal (10.5.28.166)

Host is up (0.00s latency).

MAC Address: 02:37:DC:74:C7:D4 (Unknown)

Nmap scan report for ip-10-5-29-112.ap-south-1.compute.internal (10.5.29.112)

Host is up (0.00s latency).

MAC Address: 02:1E:39:FE:FB:E6 (Unknown)

Nmap scan report for ip-10-5-30-180.ap-south-1.compute.internal (10.5.30.180)

Host is up (0.00s latency).

MAC Address: 02:10:EA:92:18:A0 (Unknown)

Nmap scan report for ip-10-5-31-114.ap-south-1.compute.internal (10.5.31.114)

Host is up (0.00s latency).

MAC Address: 02:0D:FE:AD:0C:3A (Unknown)

Nmap scan report for ip-10-5-18-27.ap-south-1.compute.internal (10.5.18.27)

Host is up.

Nmap done: 4096 IP addresses (15 hosts up) scanned in 17.37 seconds
```

Post identification of the available devices we can run the Intense scan.

```
Nmap scan report for ip-10-5-16-1.ap-south-1.compute.internal (10.5.16.1)

Host is up (0.000055s latency).

All 1000 scanned ports on ip-10-5-16-1.ap-south-1.compute.internal (10.5.16.1) are filtered

MAC Address: 02:B5:F8:B3:8D:26 (Unknown)

Too many fingerprints match this host to give specific OS details

Network Distance: 1 hop



TRACEROUTE

HOP RTT     ADDRESS

1   0.05 ms ip-10-5-16-1.ap-south-1.compute.internal (10.5.16.1)



Nmap scan report for ip-10-5-16-44.ap-south-1.compute.internal (10.5.16.44)

Host is up (0.00s latency).

Not shown: 999 filtered ports

PORT    STATE SERVICE   VERSION

443/tcp open  ssl/https

| fingerprint-strings: 

|   FourOhFourRequest: 

|     HTTP/1.1 400 Bad Request

|     Content-Length: 0

|     Date: Thu, 06 Jul 2023 07:23:54 GMT

|     Connection: close

|   GenericLines: 

|     HTTP/1.1 400 Bad Request

|     Date: Thu, 06 Jul 23 07:24:01 GMT

|     Connection: close

|     x-amz-request-id: 631FE5A4D6A8349E

|     Content-Length: 0

|   GetRequest: 

|     HTTP/1.1 400 Bad Request

|     Content-Length: 0

|     Date: Thu, 06 Jul 2023 07:23:50 GMT

|     Connection: close

|   HTTPOptions: 

|     HTTP/1.1 400 Bad Request

|     Content-Length: 0

|     Date: Thu, 06 Jul 2023 07:23:52 GMT

|     Connection: close

|   RTSPRequest: 

|     HTTP/1.1 505 HTTP Version not supported

|     Date: Thu, 06 Jul 23 07:24:03 GMT

|     Connection: close

|     x-amz-request-id: F556A179ACC9BCCF

|     Content-Length: 0

|   SIPOptions: 

|     HTTP/1.1 505 HTTP Version not supported

|     Date: Thu, 06 Jul 23 07:25:13 GMT

|     Connection: close

|     x-amz-request-id: 3EEF42811C9931FD

|_    Content-Length: 0

| http-methods: 

|_  Supported Methods: GET HEAD POST OPTIONS

|_http-title: Site doesn't have a title.

| ssl-cert: Subject: commonName=ec2messages.ap-south-1.amazonaws.com

| Subject Alternative Name: DNS:ec2messages.ap-south-1.amazonaws.com, DNS:*.ec2messages.ap-south-1.vpce.amazonaws.com

| Issuer: commonName=Amazon RSA 2048 M01/organizationName=Amazon/countryName=US

| Public Key type: rsa

| Public Key bits: 2048

| Signature Algorithm: sha256WithRSAEncryption

| Not valid before: 2023-03-16T00:00:00

| Not valid after:  2024-02-06T23:59:59

| MD5:   e304 1b6f aefd 0b21 0b0a 3bed 280f 814a

|_SHA-1: caa3 c798 f817 107e 07d1 651c 9fac 1f65 de77 2950

|_ssl-date: TLS randomness does not represent time

| tls-alpn: 

|_  http/1.1

1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :

SF-Port443-TCP:V=7.91%T=SSL%I=7%D=7/6%Time=64A66C08%P=i686-pc-windows-wind

SF:ows%r(GetRequest,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Leng

SF:th:\x200\r\nDate:\x20Thu,\x2006\x20Jul\x202023\x2007:23:50\x20GMT\r\nCo

SF:nnection:\x20close\r\n\r\n")%r(HTTPOptions,67,"HTTP/1\.1\x20400\x20Bad\

SF:x20Request\r\nContent-Length:\x200\r\nDate:\x20Thu,\x2006\x20Jul\x20202

SF:3\x2007:23:52\x20GMT\r\nConnection:\x20close\r\n\r\n")%r(FourOhFourRequ

SF:est,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Length:\x200\r\nD

SF:ate:\x20Thu,\x2006\x20Jul\x202023\x2007:23:54\x20GMT\r\nConnection:\x20

SF:close\r\n\r\n")%r(GenericLines,89,"HTTP/1\.1\x20400\x20Bad\x20Request\r

SF:\nDate:\x20Thu,\x2006\x20Jul\x2023\x2007:24:01\x20GMT\r\nConnection:\x2

SF:0close\r\nx-amz-request-id:\x20631FE5A4D6A8349E\r\nContent-Length:\x200

SF:\r\n\r\n")%r(RTSPRequest,98,"HTTP/1\.1\x20505\x20HTTP\x20Version\x20not

SF:\x20supported\r\nDate:\x20Thu,\x2006\x20Jul\x2023\x2007:24:03\x20GMT\r\

SF:nConnection:\x20close\r\nx-amz-request-id:\x20F556A179ACC9BCCF\r\nConte

SF:nt-Length:\x200\r\n\r\n")%r(SIPOptions,98,"HTTP/1\.1\x20505\x20HTTP\x20

SF:Version\x20not\x20supported\r\nDate:\x20Thu,\x2006\x20Jul\x2023\x2007:2

SF:5:13\x20GMT\r\nConnection:\x20close\r\nx-amz-request-id:\x203EEF42811C9

SF:931FD\r\nContent-Length:\x200\r\n\r\n");

MAC Address: 02:73:CE:2A:1B:90 (Unknown)

Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port

Device type: load balancer|specialized|general purpose

Running (JUST GUESSING): Citrix embedded (89%), AVtech embedded (87%), OpenBSD 4.X (86%)

OS CPE: cpe:/o:openbsd:openbsd:4.0

Aggressive OS guesses: Citrix NetScaler load balancer (89%), AVtech Room Alert 26W environmental monitor (87%), OpenBSD 4.0 (86%)

No exact OS matches for host (test conditions non-ideal).

Network Distance: 1 hop

TCP Sequence Prediction: Difficulty=260 (Good luck!)

IP ID Sequence Generation: Randomized



TRACEROUTE

HOP RTT     ADDRESS

1   0.00 ms ip-10-5-16-44.ap-south-1.compute.internal (10.5.16.44)



Nmap scan report for ip-10-5-17-93.ap-south-1.compute.internal (10.5.17.93)

Host is up.

All 1000 scanned ports on ip-10-5-17-93.ap-south-1.compute.internal (10.5.17.93) are filtered

MAC Address: 02:F6:B9:17:92:44 (Unknown)

Too many fingerprints match this host to give specific OS details

Network Distance: 1 hop



TRACEROUTE

HOP RTT ADDRESS

1   --  ip-10-5-17-93.ap-south-1.compute.internal (10.5.17.93)



Nmap scan report for ip-10-5-17-181.ap-south-1.compute.internal (10.5.17.181)

Host is up.

All 1000 scanned ports on ip-10-5-17-181.ap-south-1.compute.internal (10.5.17.181) are filtered

MAC Address: 02:B7:8C:FF:3F:F8 (Unknown)

Too many fingerprints match this host to give specific OS details

Network Distance: 1 hop



TRACEROUTE

HOP RTT ADDRESS

1   --  ip-10-5-17-181.ap-south-1.compute.internal (10.5.17.181)



Nmap scan report for ip-10-5-21-148.ap-south-1.compute.internal (10.5.21.148)

Host is up (0.00s latency).

All 1000 scanned ports on ip-10-5-21-148.ap-south-1.compute.internal (10.5.21.148) are filtered

MAC Address: 02:5E:4D:80:7A:6C (Unknown)

Too many fingerprints match this host to give specific OS details

Network Distance: 1 hop



TRACEROUTE

HOP RTT     ADDRESS

1   0.00 ms ip-10-5-21-148.ap-south-1.compute.internal (10.5.21.148)



Nmap scan report for ip-10-5-23-125.ap-south-1.compute.internal (10.5.23.125)

Host is up (0.00s latency).

All 1000 scanned ports on ip-10-5-23-125.ap-south-1.compute.internal (10.5.23.125) are filtered

MAC Address: 02:CF:62:8A:77:CE (Unknown)

Too many fingerprints match this host to give specific OS details

Network Distance: 1 hop



TRACEROUTE

HOP RTT     ADDRESS

1   0.00 ms ip-10-5-23-125.ap-south-1.compute.internal (10.5.23.125)



Nmap scan report for ip-10-5-26-246.ap-south-1.compute.internal (10.5.26.246)

Host is up (0.00s latency).

All 1000 scanned ports on ip-10-5-26-246.ap-south-1.compute.internal (10.5.26.246) are filtered

MAC Address: 02:CF:F7:C0:97:FA (Unknown)

Too many fingerprints match this host to give specific OS details

Network Distance: 1 hop



TRACEROUTE

HOP RTT     ADDRESS

1   0.00 ms ip-10-5-26-246.ap-south-1.compute.internal (10.5.26.246)



Nmap scan report for ip-10-5-28-45.ap-south-1.compute.internal (10.5.28.45)

Host is up (0.0000060s latency).

Not shown: 999 filtered ports

PORT    STATE SERVICE   VERSION

443/tcp open  ssl/https

| fingerprint-strings: 

|   FourOhFourRequest: 

|     HTTP/1.1 500 Internal Server Error

|     Content-Length: 0

|     Date: Thu, 06 Jul 2023 07:23:54 GMT

|     Connection: close

|   GenericLines: 

|     HTTP/1.1 400 Bad Request

|     Date: Thu, 06 Jul 23 07:24:00 GMT

|     Connection: close

|     x-amz-request-id: 4B9AD7A3B485150D

|     Content-Length: 0

|   GetRequest: 

|     HTTP/1.1 500 Internal Server Error

|     Content-Length: 0

|     Date: Thu, 06 Jul 2023 07:23:49 GMT

|     Connection: close

|   HTTPOptions: 

|     HTTP/1.1 500 Internal Server Error

|     Content-Length: 0

|     Date: Thu, 06 Jul 2023 07:23:52 GMT

|     Connection: close

|   RTSPRequest: 

|     HTTP/1.1 505 HTTP Version not supported

|     Date: Thu, 06 Jul 23 07:24:03 GMT

|     Connection: close

|     x-amz-request-id: D02315B81C453241

|     Content-Length: 0

|   SIPOptions: 

|     HTTP/1.1 505 HTTP Version not supported

|     Date: Thu, 06 Jul 23 07:25:13 GMT

|     Connection: close

|     x-amz-request-id: DD28E485045A572B

|_    Content-Length: 0

|_http-cors: HEAD GET POST PUT DELETE TRACE OPTIONS CONNECT PATCH

| http-methods: 

|_  Supported Methods: GET HEAD POST OPTIONS

|_http-title: Site doesn't have a title.

| ssl-cert: Subject: commonName=ssm.ap-south-1.amazonaws.com

| Subject Alternative Name: DNS:ssm.ap-south-1.amazonaws.com, DNS:*.ssm.ap-south-1.vpce.amazonaws.com, DNS:legacy.ssm.ap-south-1.amazonaws.com

| Issuer: commonName=Amazon RSA 2048 M01/organizationName=Amazon/countryName=US

| Public Key type: rsa

| Public Key bits: 2048

| Signature Algorithm: sha256WithRSAEncryption

| Not valid before: 2023-03-08T00:00:00

| Not valid after:  2024-02-10T23:59:59

| MD5:   1144 08af a35e b070 787a 244f d403 5fb5

|_SHA-1: 84ce 79f8 de97 2f79 f1bd f9cb ed1f c95b 717d 3b45

|_ssl-date: TLS randomness does not represent time

| tls-alpn: 

|_  http/1.1

1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :

SF-Port443-TCP:V=7.91%T=SSL%I=7%D=7/6%Time=64A66C08%P=i686-pc-windows-wind

SF:ows%r(GetRequest,71,"HTTP/1\.1\x20500\x20Internal\x20Server\x20Error\r\

SF:nContent-Length:\x200\r\nDate:\x20Thu,\x2006\x20Jul\x202023\x2007:23:49

SF:\x20GMT\r\nConnection:\x20close\r\n\r\n")%r(HTTPOptions,71,"HTTP/1\.1\x

SF:20500\x20Internal\x20Server\x20Error\r\nContent-Length:\x200\r\nDate:\x

SF:20Thu,\x2006\x20Jul\x202023\x2007:23:52\x20GMT\r\nConnection:\x20close\

SF:r\n\r\n")%r(FourOhFourRequest,71,"HTTP/1\.1\x20500\x20Internal\x20Serve

SF:r\x20Error\r\nContent-Length:\x200\r\nDate:\x20Thu,\x2006\x20Jul\x20202

SF:3\x2007:23:54\x20GMT\r\nConnection:\x20close\r\n\r\n")%r(GenericLines,8

SF:9,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nDate:\x20Thu,\x2006\x20Jul\x20

SF:23\x2007:24:00\x20GMT\r\nConnection:\x20close\r\nx-amz-request-id:\x204

SF:B9AD7A3B485150D\r\nContent-Length:\x200\r\n\r\n")%r(RTSPRequest,98,"HTT

SF:P/1\.1\x20505\x20HTTP\x20Version\x20not\x20supported\r\nDate:\x20Thu,\x

SF:2006\x20Jul\x2023\x2007:24:03\x20GMT\r\nConnection:\x20close\r\nx-amz-r

SF:equest-id:\x20D02315B81C453241\r\nContent-Length:\x200\r\n\r\n")%r(SIPO

SF:ptions,98,"HTTP/1\.1\x20505\x20HTTP\x20Version\x20not\x20supported\r\nD

SF:ate:\x20Thu,\x2006\x20Jul\x2023\x2007:25:13\x20GMT\r\nConnection:\x20cl

SF:ose\r\nx-amz-request-id:\x20DD28E485045A572B\r\nContent-Length:\x200\r\

SF:n\r\n");

MAC Address: 02:DF:10:BD:81:F6 (Unknown)

Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port

Device type: load balancer|specialized

Running (JUST GUESSING): Citrix embedded (89%), AVtech embedded (87%)

Aggressive OS guesses: Citrix NetScaler load balancer (89%), AVtech Room Alert 26W environmental monitor (87%)

No exact OS matches for host (test conditions non-ideal).

Network Distance: 1 hop

TCP Sequence Prediction: Difficulty=259 (Good luck!)

IP ID Sequence Generation: Randomized



TRACEROUTE

HOP RTT     ADDRESS

1   0.01 ms ip-10-5-28-45.ap-south-1.compute.internal (10.5.28.45)



Nmap scan report for ip-10-5-28-166.ap-south-1.compute.internal (10.5.28.166)

Host is up (0.00s latency).

Not shown: 993 filtered ports

PORT      STATE SERVICE            VERSION

80/tcp    open  http               HttpFileServer httpd 2.3

|_http-favicon: Unknown favicon MD5: 759792EDD4EF8E6BC2D1877D27153CB1

| http-methods: 

|_  Supported Methods: GET HEAD POST

|_http-server-header: HFS 2.3

|_http-title: HFS /

135/tcp   open  msrpc              Microsoft Windows RPC

139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn

445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds

3389/tcp  open  ssl/ms-wbt-server?

| rdp-ntlm-info: 

|   Target_Name: HTTP-SERVER

|   NetBIOS_Domain_Name: HTTP-SERVER

|   NetBIOS_Computer_Name: HTTP-SERVER

|   DNS_Domain_Name: http-server

|   DNS_Computer_Name: http-server

|   Product_Version: 6.3.9600

|_  System_Time: 2023-07-06T07:26:12+00:00

| ssl-cert: Subject: commonName=http-server

| Issuer: commonName=http-server

| Public Key type: rsa

| Public Key bits: 2048

| Signature Algorithm: sha256WithRSAEncryption

| Not valid before: 2023-07-05T07:14:24

| Not valid after:  2024-01-04T07:14:24

| MD5:   4b6d d3c0 39bb b277 79d7 800c 13a0 ad0c

|_SHA-1: d312 0356 e350 52da a414 6bf6 2c11 5c73 de26 07a6

49154/tcp open  msrpc              Microsoft Windows RPC

49155/tcp open  msrpc              Microsoft Windows RPC

MAC Address: 02:37:DC:74:C7:D4 (Unknown)

Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port

Device type: general purpose

Running (JUST GUESSING): Microsoft Windows 2012 (89%)

OS CPE: cpe:/o:microsoft:windows_server_2012

Aggressive OS guesses: Microsoft Windows Server 2012 (89%), Microsoft Windows Server 2012 or Windows Server 2012 R2 (89%), Microsoft Windows Server 2012 R2 (89%)

No exact OS matches for host (test conditions non-ideal).

Uptime guess: 0.009 days (since Thu Jul 06 07:14:11 2023)

Network Distance: 1 hop

TCP Sequence Prediction: Difficulty=261 (Good luck!)

IP ID Sequence Generation: Incremental

Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows



Host script results:

| nbstat: NetBIOS name: HTTP-SERVER, NetBIOS user: <unknown>, NetBIOS MAC: 02:37:dc:74:c7:d4 (unknown)

| Names:

|   HTTP-SERVER<00>      Flags: <unique><active>

|   WORKGROUP<00>        Flags: <group><active>

|_  HTTP-SERVER<20>      Flags: <unique><active>

| smb-security-mode: 

|   authentication_level: user

|   challenge_response: supported

|_  message_signing: disabled (dangerous, but default)

| smb2-security-mode: 

|   2.02: 

|_    Message signing enabled but not required

| smb2-time: 

|   date: 2023-07-06T07:26:12

|_  start_date: 2023-07-06T07:14:23



TRACEROUTE

HOP RTT     ADDRESS

1   0.00 ms ip-10-5-28-166.ap-south-1.compute.internal (10.5.28.166)



Nmap scan report for ip-10-5-28-190.ap-south-1.compute.internal (10.5.28.190)

Host is up (0.00s latency).

All 1000 scanned ports on ip-10-5-28-190.ap-south-1.compute.internal (10.5.28.190) are filtered

MAC Address: 02:2C:B3:35:C0:0C (Unknown)

Too many fingerprints match this host to give specific OS details

Network Distance: 1 hop



TRACEROUTE

HOP RTT     ADDRESS

1   0.00 ms ip-10-5-28-190.ap-south-1.compute.internal (10.5.28.190)



Nmap scan report for ip-10-5-30-180.ap-south-1.compute.internal (10.5.30.180)

Host is up (0.00s latency).

All 1000 scanned ports on ip-10-5-30-180.ap-south-1.compute.internal (10.5.30.180) are filtered

MAC Address: 02:10:EA:92:18:A0 (Unknown)

Too many fingerprints match this host to give specific OS details

Network Distance: 1 hop



TRACEROUTE

HOP RTT     ADDRESS

1   0.00 ms ip-10-5-30-180.ap-south-1.compute.internal (10.5.30.180)



Nmap scan report for ip-10-5-31-114.ap-south-1.compute.internal (10.5.31.114)

Host is up (0.00s latency).

All 1000 scanned ports on ip-10-5-31-114.ap-south-1.compute.internal (10.5.31.114) are filtered

MAC Address: 02:0D:FE:AD:0C:3A (Unknown)

Too many fingerprints match this host to give specific OS details

Network Distance: 1 hop



TRACEROUTE

HOP RTT     ADDRESS

1   0.00 ms ip-10-5-31-114.ap-south-1.compute.internal (10.5.31.114)



Initiating SYN Stealth Scan at 07:26

Scanning ip-10-5-18-27.ap-south-1.compute.internal (10.5.18.27) [1000 ports]

Discovered open port 135/tcp on 10.5.18.27

Discovered open port 3389/tcp on 10.5.18.27

Discovered open port 445/tcp on 10.5.18.27

Discovered open port 139/tcp on 10.5.18.27

Completed SYN Stealth Scan at 07:26, 0.09s elapsed (1000 total ports)

Initiating Service scan at 07:26

Scanning 4 services on ip-10-5-18-27.ap-south-1.compute.internal (10.5.18.27)

Completed Service scan at 07:26, 6.02s elapsed (4 services on 1 host)

Initiating OS detection (try #1) against ip-10-5-18-27.ap-south-1.compute.internal (10.5.18.27)

Retrying OS detection (try #2) against ip-10-5-18-27.ap-south-1.compute.internal (10.5.18.27)

Retrying OS detection (try #3) against ip-10-5-18-27.ap-south-1.compute.internal (10.5.18.27)

Retrying OS detection (try #4) against ip-10-5-18-27.ap-south-1.compute.internal (10.5.18.27)

Retrying OS detection (try #5) against ip-10-5-18-27.ap-south-1.compute.internal (10.5.18.27)

NSE: Script scanning 10.5.18.27.

Initiating NSE at 07:27

Completed NSE at 07:27, 12.29s elapsed

Initiating NSE at 07:27

Completed NSE at 07:27, 0.03s elapsed

Initiating NSE at 07:27

Completed NSE at 07:27, 0.00s elapsed

Nmap scan report for ip-10-5-18-27.ap-south-1.compute.internal (10.5.18.27)

Host is up (0.00s latency).

Not shown: 996 closed ports

PORT     STATE SERVICE       VERSION

135/tcp  open  msrpc         Microsoft Windows RPC

139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn

445/tcp  open  microsoft-ds?

3389/tcp open  ms-wbt-server Microsoft Terminal Services

| rdp-ntlm-info: 

|   Target_Name: ATTACKDEFENSE

|   NetBIOS_Domain_Name: ATTACKDEFENSE

|   NetBIOS_Computer_Name: ATTACKDEFENSE

|   DNS_Domain_Name: AttackDefense

|   DNS_Computer_Name: AttackDefense

|   Product_Version: 10.0.17763

|_  System_Time: 2023-07-06T07:27:09+00:00

| ssl-cert: Subject: commonName=AttackDefense

| Issuer: commonName=AttackDefense

| Public Key type: rsa

| Public Key bits: 2048

| Signature Algorithm: sha256WithRSAEncryption

| Not valid before: 2023-07-05T07:14:33

| Not valid after:  2024-01-04T07:14:33

| MD5:   cdac 3966 6666 e83a 6f0d dcb6 0575 87ad

|_SHA-1: 33a8 e974 9ca2 5f7e 94a6 7e36 9a4a 80bb a3aa 2496

|_ssl-date: 2023-07-06T07:27:21+00:00; 0s from scanner time.

No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).

TCP/IP fingerprint:

OS:SCAN(V=7.91%E=4%D=7/6%OT=135%CT=1%CU=38753%PV=Y%DS=0%DC=L%G=Y%TM=64A66CD

OS:9%P=i686-pc-windows-windows)SEQ(SP=105%GCD=1%ISR=103%TI=I%CI=I%II=I%SS=S

OS:%TS=U)OPS(O1=MFFD7NW8NNS%O2=MFFD7NW8NNS%O3=MFFD7NW8%O4=MFFD7NW8NNS%O5=MF

OS:FD7NW8NNS%O6=MFFD7NNS)WIN(W1=FFFF%W2=FFFF%W3=FFFF%W4=FFFF%W5=FFFF%W6=FF7

OS:0)ECN(R=Y%DF=Y%T=80%W=FFFF%O=MFFD7NW8NNS%CC=Y%Q=)T1(R=Y%DF=Y%T=80%S=O%A=

OS:S+%F=AS%RD=0%Q=)T2(R=Y%DF=Y%T=80%W=0%S=Z%A=S%F=AR%O=%RD=0%Q=)T3(R=Y%DF=Y

OS:%T=80%W=0%S=Z%A=O%F=AR%O=%RD=0%Q=)T4(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=%RD

OS:=0%Q=)T5(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=80%W=0

OS:%S=A%A=O%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1

OS:(R=Y%DF=N%T=80%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=Z%RUCK=G%RUD=G)IE(R=Y%DFI

OS:=N%T=80%CD=Z)



Network Distance: 0 hops

TCP Sequence Prediction: Difficulty=261 (Good luck!)

IP ID Sequence Generation: Incremental

Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows



Host script results:

| smb2-security-mode: 

|   2.02: 

|_    Message signing enabled but not required

| smb2-time: 

|   date: 2023-07-06T07:27:12

|_  start_date: N/A



NSE: Script Post-scanning.

Initiating NSE at 07:27

Completed NSE at 07:27, 0.00s elapsed

Initiating NSE at 07:27

Completed NSE at 07:27, 0.00s elapsed

Initiating NSE at 07:27

Completed NSE at 07:27, 0.00s elapsed

Read data files from: C:\Program Files (x86)\Nmap

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .

Nmap done: 4096 IP addresses (13 hosts up) scanned in 257.94 seconds

           Raw packets sent: 33999 (1.423MB) | Rcvd: 2368 (107.413KB)


```
