In this lab, run the following **auxiliary modules** again the target:

- auxiliary/scanner/http/apache_userdir_enum
- auxiliary/scanner/http/brute_dirs
- auxiliary/scanner/http/dir_scanner
- auxiliary/scanner/http/dir_listing
- auxiliary/scanner/http/http_put
- auxiliary/scanner/http/files_dir
- auxiliary/scanner/http/http_login
- auxiliary/scanner/http/http_header
- auxiliary/scanner/http/http_version
- auxiliary/scanner/http/robots_txt

[[ifconfig]]

```
root@attackdefense:~# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.1.0.9  netmask 255.255.0.0  broadcast 10.1.255.255
        ether 02:42:0a:01:00:09  txqueuelen 0  (Ethernet)
        RX packets 166  bytes 14117 (13.7 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 117  bytes 314988 (307.6 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.166.52.2  netmask 255.255.255.0  broadcast 192.166.52.255
        ether 02:42:c0:a6:34:02  txqueuelen 0  (Ethernet)
        RX packets 24  bytes 2060 (2.0 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 7  bytes 574 (574.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 18  bytes 1656 (1.6 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 18  bytes 1656 (1.6 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

[[ip]]

```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
14325: eth0@if14326: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:09 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.9/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
14328: eth1@if14329: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:a6:34:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.166.52.2/24 brd 192.166.52.255 scope global eth1
       valid_lft forever preferred_lft forever
```

[[ping]]

```
root@attackdefense:~# ping -c 5 192.166.52.3
PING 192.166.52.3 (192.166.52.3) 56(84) bytes of data.
64 bytes from 192.166.52.3: icmp_seq=1 ttl=64 time=0.077 ms
64 bytes from 192.166.52.3: icmp_seq=2 ttl=64 time=0.043 ms
64 bytes from 192.166.52.3: icmp_seq=3 ttl=64 time=0.040 ms
64 bytes from 192.166.52.3: icmp_seq=4 ttl=64 time=0.039 ms
64 bytes from 192.166.52.3: icmp_seq=5 ttl=64 time=0.045 ms

--- 192.166.52.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 88ms
rtt min/avg/max/mdev = 0.039/0.048/0.077/0.016 ms
```

[[metasploit]]

```
root@attackdefense:~# service postgresql start && msfconsole
[ ok ] Starting PostgreSQL 11 database server: main.
       =[ metasploit v5.0.18-dev                          ]
+ -- --=[ 1882 exploits - 1062 auxiliary - 328 post       ]
+ -- --=[ 546 payloads - 44 encoders - 10 nops            ]
+ -- --=[ 2 evasion                                       ]

msf5 > db_status
[*] Connected to msf. Connection type: postgresql.
msf5 > 
```

Creating a new workspace

```
msf5 > workspace -a http_enum
[*] Added workspace: http_enum
[*] Workspace: http_enum
msf5 > workspace
  default
* http_enum
msf5 > 
```

searching for the auxiliary module related to http

```
msf5 > search type:auxiliary name:http

Matching Modules
================

   #   Name                                                     Disclosure Date  Rank    Check  Description
   -   ----                                                     ---------------  ----    -----  -----------
   1   auxiliary/admin/http/intersil_pass_reset                 2007-09-10       normal  Yes    Intersil (Boa) HTTPd Basic Authentication Password Reset
   2   auxiliary/admin/http/scrutinizer_add_user                2012-07-27       normal  No     Plixer Scrutinizer NetFlow and sFlow Analyzer HTTP Authentication Bypass
   3   auxiliary/dos/cisco/ios_http_percentpercent              2000-04-26       normal  No     Cisco IOS HTTP GET /%% Request Denial of Service
   4   auxiliary/dos/http/brother_debut_dos                     2017-11-02       normal  No     Brother Debut http Denial Of Service
   5   auxiliary/dos/http/flexense_http_server_dos              2018-03-09       normal  Yes    Flexense HTTP Server Denial Of Service
   6   auxiliary/dos/http/monkey_headers                        2013-05-30       normal  No     Monkey HTTPD Header Parsing Denial of Service (DoS)
   7   auxiliary/dos/http/ms15_034_ulonglongadd                                  normal  Yes    MS15-034 HTTP Protocol Stack Request Handling Denial-of-Service
   8   auxiliary/dos/http/nodejs_pipelining                     2013-10-18       normal  Yes    Node.js HTTP Pipelining Denial of Service
   9   auxiliary/dos/http/webrick_regex                         2008-08-08       normal  No     Ruby WEBrick::HTTP::DefaultFileHandler DoS
   10  auxiliary/fuzzers/http/http_form_field                                    normal  No     HTTP Form Field Fuzzer
   11  auxiliary/fuzzers/http/http_get_uri_long                                  normal  No     HTTP GET Request URI Fuzzer (Incrementing Lengths)
   12  auxiliary/fuzzers/http/http_get_uri_strings                               normal  No     HTTP GET Request URI Fuzzer (Fuzzer Strings)
   13  auxiliary/gather/apple_safari_ftp_url_cookie_theft       2015-04-08       normal  No     Apple OSX/iOS/Windows Safari Non-HTTPOnly Cookie Theft
   14  auxiliary/gather/browser_info                            2016-03-22       normal  No     HTTP Client Information Gather
   15  auxiliary/gather/browser_lanipleak                       2013-09-05       normal  No     HTTP Client LAN IP Address Gather
   16  auxiliary/gather/impersonate_ssl                                          normal  No     HTTP SSL Certificate Impersonation
   17  auxiliary/scanner/http/backup_file                                        normal  Yes    HTTP Backup File Scanner
   18  auxiliary/scanner/http/blind_sql_query                                    normal  Yes    HTTP Blind SQL Injection Scanner
   19  auxiliary/scanner/http/brute_dirs                                         normal  Yes    HTTP Directory Brute Force Scanner
   20  auxiliary/scanner/http/cert                                               normal  Yes    HTTP SSL Certificate Checker
   21  auxiliary/scanner/http/cisco_device_manager              2000-10-26       normal  Yes    Cisco Device HTTP Device Manager Access
   22  auxiliary/scanner/http/cisco_ios_auth_bypass             2001-06-27       normal  Yes    Cisco IOS HTTP Unauthorized Administrative Access
   23  auxiliary/scanner/http/copy_of_file                                       normal  Yes    HTTP Copy File Scanner
   24  auxiliary/scanner/http/dir_listing                                        normal  Yes    HTTP Directory Listing Scanner
   25  auxiliary/scanner/http/dir_scanner                                        normal  Yes    HTTP Directory Scanner
   26  auxiliary/scanner/http/dlink_dir_300_615_http_login                       normal  Yes    D-Link DIR-300A / DIR-320 / DIR-615D HTTP Login Utility
   27  auxiliary/scanner/http/dlink_dir_615h_http_login                          normal  Yes    D-Link DIR-615H HTTP Login Utility
   28  auxiliary/scanner/http/dlink_dir_session_cgi_http_login                   normal  Yes    D-Link DIR-300B / DIR-600B / DIR-815 / DIR-645 HTTP Login Utility
   29  auxiliary/scanner/http/error_sql_injection                                normal  Yes    HTTP Error Based SQL Injection Scanner
   30  auxiliary/scanner/http/f5_bigip_virtual_server                            normal  Yes    F5 BigIP HTTP Virtual Server Scanner
   31  auxiliary/scanner/http/file_same_name_dir                                 normal  Yes    HTTP File Same Name Directory Scanner
   32  auxiliary/scanner/http/files_dir                                          normal  Yes    HTTP Interesting File Scanner
   33  auxiliary/scanner/http/git_scanner                                        normal  Yes    HTTP Git Scanner
   34  auxiliary/scanner/http/groupwise_agents_http_traversal                    normal  Yes    Novell Groupwise Agents HTTP Directory Traversal
   35  auxiliary/scanner/http/host_header_injection                              normal  Yes    HTTP Host Header Injection Detection
   36  auxiliary/scanner/http/http_header                                        normal  Yes    HTTP Header Detection
   37  auxiliary/scanner/http/http_hsts                                          normal  Yes    HTTP Strict Transport Security (HSTS) Detection
   38  auxiliary/scanner/http/http_login                                         normal  Yes    HTTP Login Utility
   39  auxiliary/scanner/http/http_put                                           normal  Yes    HTTP Writable Path PUT/DELETE File Access
   40  auxiliary/scanner/http/http_sickrage_password_leak       2018-03-08       normal  No     HTTP SickRage Password Leak
   41  auxiliary/scanner/http/http_traversal                                     normal  Yes    Generic HTTP Directory Traversal Utility
   42  auxiliary/scanner/http/http_version                                       normal  Yes    HTTP Version Detection
   43  auxiliary/scanner/http/httpbl_lookup                                      normal  Yes    Http:BL Lookup
   44  auxiliary/scanner/http/httpdasm_directory_traversal                       normal  No     Httpdasm Directory Traversal
   45  auxiliary/scanner/http/iis_internal_ip                                    normal  Yes    Microsoft IIS HTTP Internal IP Disclosure
   46  auxiliary/scanner/http/lucky_punch                                        normal  Yes    HTTP Microsoft SQL Injection Table XSS Infection
   47  auxiliary/scanner/http/mod_negotiation_brute                              normal  Yes    Apache HTTPD mod_negotiation Filename Bruter
   48  auxiliary/scanner/http/mod_negotiation_scanner                            normal  Yes    Apache HTTPD mod_negotiation Scanner
   49  auxiliary/scanner/http/ms15_034_http_sys_memory_dump                      normal  Yes    MS15-034 HTTP Protocol Stack Request Handling HTTP.SYS Memory Information Disclosure
   50  auxiliary/scanner/http/open_proxy                                         normal  Yes    HTTP Open Proxy Detection
   51  auxiliary/scanner/http/options                                            normal  Yes    HTTP Options Detection
   52  auxiliary/scanner/http/owa_iis_internal_ip               2012-12-17       normal  Yes    Outlook Web App (OWA) / Client Access Server (CAS) IIS HTTP Internal IP Disclosure
   53  auxiliary/scanner/http/prev_dir_same_name_file                            normal  Yes    HTTP Previous Directory File Scanner
   54  auxiliary/scanner/http/replace_ext                                        normal  Yes    HTTP File Extension Scanner
   55  auxiliary/scanner/http/robots_txt                                         normal  Yes    HTTP Robots.txt Content Scanner
   56  auxiliary/scanner/http/scraper                                            normal  Yes    HTTP Page Scraper
   57  auxiliary/scanner/http/soap_xml                                           normal  Yes    HTTP SOAP Verb/Noun Brute Force Scanner
   58  auxiliary/scanner/http/ssl                                                normal  Yes    HTTP SSL Certificate Information
   59  auxiliary/scanner/http/ssl_version                       2014-10-14       normal  Yes    HTTP SSL/TLS Version Detection (POODLE scanner)
   60  auxiliary/scanner/http/svn_scanner                                        normal  Yes    HTTP Subversion Scanner
   61  auxiliary/scanner/http/title                                              normal  Yes    HTTP HTML Title Tag Content Grabber
   62  auxiliary/scanner/http/trace                                              normal  Yes    HTTP Cross-Site Tracing Detection
   63  auxiliary/scanner/http/trace_axd                                          normal  Yes    HTTP trace.axd Content Scanner
   64  auxiliary/scanner/http/verb_auth_bypass                                   normal  Yes    HTTP Verb Authentication Bypass Scanner
   65  auxiliary/scanner/http/vhost_scanner                                      normal  Yes    HTTP Virtual Host Brute Force Scanner
   66  auxiliary/scanner/http/web_vulndb                                         normal  Yes    HTTP Vuln Scanner
   67  auxiliary/scanner/http/webdav_internal_ip                                 normal  Yes    HTTP WebDAV Internal IP Scanner
   68  auxiliary/scanner/http/webdav_scanner                                     normal  Yes    HTTP WebDAV Scanner
   69  auxiliary/scanner/http/webdav_website_content                             normal  Yes    HTTP WebDAV Website Content Scanner
   70  auxiliary/scanner/http/xpath                                              normal  Yes    HTTP Blind XPATH 1.0 Injector
   71  auxiliary/server/browser_autopwn                                          normal  No     HTTP Client Automatic Exploiter
   72  auxiliary/server/browser_autopwn2                        2015-07-05       normal  No     HTTP Client Automatic Exploiter 2 (Browser Autopwn)
   73  auxiliary/server/capture/http                                             normal  No     Authentication Capture: HTTP
   74  auxiliary/server/capture/http_basic                                       normal  No     HTTP Client Basic Authentication Credential Collector
   75  auxiliary/server/capture/http_javascript_keylogger                        normal  No     Capture: HTTP JavaScript Keylogger
   76  auxiliary/server/capture/http_ntlm                                        normal  No     HTTP Client MS Credential Catcher
   77  auxiliary/server/http_ntlmrelay 
```

Setting the global variable 

```
msf5 > setg rhosts 192.166.52.3
rhosts => 192.166.52.3
```

http version:

```
msf5 > use auxiliary/scanner/http/http_version 
msf5 auxiliary(scanner/http/http_version) > options

Module options (auxiliary/scanner/http/http_version):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   Proxies                   no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS   192.166.52.3     yes       The target address range or CIDR identifier
   RPORT    80               yes       The target port (TCP)
   SSL      false            no        Negotiate SSL/TLS for outgoing connections
   THREADS  1                yes       The number of concurrent threads
   VHOST                     no        HTTP server virtual host

msf5 auxiliary(scanner/http/http_version) > run

[+] 192.166.52.3:80 Apache/2.4.18 (Ubuntu)
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

headers information:

```
msf5 auxiliary(scanner/http/http_version) > use auxiliary/scanner/http/http_header
msf5 auxiliary(scanner/http/http_header) > options

Module options (auxiliary/scanner/http/http_header):

   Name         Current Setting                                                        Required  Description
   ----         ---------------                                                        --------  -----------
   HTTP_METHOD  HEAD                                                                   yes       HTTP Method to use, HEAD or GET (Accepted: GET, HEAD)
   IGN_HEADER   Vary,Date,Content-Length,Connection,Etag,Expires,Pragma,Accept-Ranges  yes       List of headers to ignore, seperated by comma
   Proxies                                                                             no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS       192.166.52.3                                                           yes       The target address range or CIDR identifier
   RPORT        80                                                                     yes       The target port (TCP)
   SSL          false                                                                  no        Negotiate SSL/TLS for outgoing connections
   TARGETURI    /                                                                      yes       The URI to use
   THREADS      1                                                                      yes       The number of concurrent threads
   VHOST                                                                               no        HTTP server virtual host

msf5 auxiliary(scanner/http/http_header) > run

[+] 192.166.52.3:80      : CONTENT-TYPE: text/html
[+] 192.166.52.3:80      : LAST-MODIFIED: Wed, 27 Feb 2019 04:21:01 GMT
[+] 192.166.52.3:80      : SERVER: Apache/2.4.18 (Ubuntu)
[+] 192.166.52.3:80      : detected 3 headers
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

robots_txt:

```
msf5 auxiliary(scanner/http/http_header) > use auxiliary/scanner/http/robots_txt
msf5 auxiliary(scanner/http/robots_txt) > options

Module options (auxiliary/scanner/http/robots_txt):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   PATH     /                yes       The test path to find robots.txt file
   Proxies                   no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS   192.166.52.3     yes       The target address range or CIDR identifier
   RPORT    80               yes       The target port (TCP)
   SSL      false            no        Negotiate SSL/TLS for outgoing connections
   THREADS  1                yes       The number of concurrent threads
   VHOST                     no        HTTP server virtual host

msf5 auxiliary(scanner/http/robots_txt) > run

[*] [192.166.52.3] /robots.txt found
[+] Contents of Robots.txt:
# robots.txt for attackdefense 
User-agent: test                     
# Directories
Allow: /webmail

User-agent: *
# Directories
Disallow: /data
Disallow: /secure

[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

curl and see if we can find anything in the directories:

```
msf5 auxiliary(scanner/http/robots_txt) > curl http://192.166.52.3/data
[*] exec: curl http://192.166.52.3/data

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   311  100   311    0     0   303k      0 --:--:-- --:--:-- --:--:--  303k
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="http://192.166.52.3/data/">here</a>.</p>
<hr>
<address>Apache/2.4.18 (Ubuntu) Server at 192.166.52.3 Port 80</address>
</body></html>
msf5 auxiliary(scanner/http/robots_txt) > curl http://192.166.52.3/secure
[*] exec: curl http://192.166.52.3/secure

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   459  100   459    0     0   448k      0 --:--:-- --:--:-- --:--:--  448k
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>401 Unauthorized</title>
</head><body>
<h1>Unauthorized</h1>
<p>This server could not verify that you
are authorized to access the document
requested.  Either you supplied the wrong
credentials (e.g., bad password), or your
browser doesn't understand how to supply
the credentials required.</p>
<hr>
<address>Apache/2.4.18 (Ubuntu) Server at 192.166.52.3 Port 80</address>
</body></html>
```

Let us try to see if we do have anymore directories available apart from the directories mentioned in robots

```
msf5 auxiliary(scanner/http/robots_txt) > use auxiliary/scanner/http/brute_dirs
msf5 auxiliary(scanner/http/brute_dirs) > options

Module options (auxiliary/scanner/http/brute_dirs):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   FORMAT   a,aa,aaa         yes       The expected directory format (a alpha, d digit, A upperalpha)
   PATH     /                yes       The path to identify directories
   Proxies                   no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS   192.166.52.3     yes       The target address range or CIDR identifier
   RPORT    80               yes       The target port (TCP)
   SSL      false            no        Negotiate SSL/TLS for outgoing connections
   THREADS  1                yes       The number of concurrent threads
   VHOST                     no        HTTP server virtual host

msf5 auxiliary(scanner/http/brute_dirs) > run

[*] Using code '404' as not found.
[+] Found http://192.166.52.3:80/doc/ 200
[+] Found http://192.166.52.3:80/pro/ 200
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

```
msf5 auxiliary(scanner/http/brute_dirs) > use auxiliary/scanner/http/dir_scanner
msf5 auxiliary(scanner/http/dir_scanner) > options

Module options (auxiliary/scanner/http/dir_scanner):

   Name        Current Setting                                          Required  Description
   ----        ---------------                                          --------  -----------
   DICTIONARY  /usr/share/metasploit-framework/data/wmap/wmap_dirs.txt  no        Path of word dictionary to use
   PATH        /                                                        yes       The path  to identify files
   Proxies                                                              no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS      192.166.52.3                                             yes       The target address range or CIDR identifier
   RPORT       80                                                       yes       The target port (TCP)
   SSL         false                                                    no        Negotiate SSL/TLS for outgoing connections
   THREADS     1                                                        yes       The number of concurrent threads
   VHOST                                                                no        HTTP server virtual host

msf5 auxiliary(scanner/http/dir_scanner) > run

[*] Detecting error code
[*] Using code '404' as not found for 192.166.52.3
[+] Found http://192.166.52.3:80/cgi-bin/ 404 (192.166.52.3)
[+] Found http://192.166.52.3:80/data/ 404 (192.166.52.3)
[+] Found http://192.166.52.3:80/downloads/ 200 (192.166.52.3)
[+] Found http://192.166.52.3:80/doc/ 200 (192.166.52.3)
[+] Found http://192.166.52.3:80/icons/ 404 (192.166.52.3)
[+] Found http://192.166.52.3:80/manual/ 404 (192.166.52.3)
[+] Found http://192.166.52.3:80/secure/ 404 (192.166.52.3)
[+] Found http://192.166.52.3:80/uploads/ 404 (192.166.52.3)
[+] Found http://192.166.52.3:80/users/ 404 (192.166.52.3)
[+] Found http://192.166.52.3:80/view/ 200 (192.166.52.3)
[+] Found http://192.166.52.3:80/webadmin/ 200 (192.166.52.3)
[+] Found http://192.166.52.3:80/web_app/ 200 (192.166.52.3)
[+] Found http://192.166.52.3:80/webdb/ 404 (192.166.52.3)
[+] Found http://192.166.52.3:80/webdav/ 404 (192.166.52.3)
[+] Found http://192.166.52.3:80/webmail/ 404 (192.166.52.3)
[+] Found http://192.166.52.3:80/~nobody/ 404 (192.166.52.3)
[+] Found http://192.166.52.3:80/~admin/ 404 (192.166.52.3)
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

```
msf5 auxiliary(scanner/http/dir_scanner) > use auxiliary/scanner/http/files_dir
msf5 auxiliary(scanner/http/files_dir) > options

Module options (auxiliary/scanner/http/files_dir):

   Name        Current Setting                                           Required  Description
   ----        ---------------                                           --------  -----------
   DICTIONARY  /usr/share/metasploit-framework/data/wmap/wmap_files.txt  no        Path of word dictionary to use
   EXT                                                                   no        Append file extension to use
   PATH        /                                                         yes       The path  to identify files
   Proxies                                                               no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS      192.166.52.3                                              yes       The target address range or CIDR identifier
   RPORT       80                                                        yes       The target port (TCP)
   SSL         false                                                     no        Negotiate SSL/TLS for outgoing connections
   THREADS     1                                                         yes       The number of concurrent threads
   VHOST                                                                 no        HTTP server virtual host

msf5 auxiliary(scanner/http/files_dir) > run

[*] Using code '404' as not found for files with extension .null
[*] Using code '404' as not found for files with extension .backup
[+] Found http://192.166.52.3:80/file.backup 200
[*] Using code '404' as not found for files with extension .bak
[*] Using code '404' as not found for files with extension .c
[+] Found http://192.166.52.3:80/code.c 200
[*] Using code '404' as not found for files with extension .cfg
[+] Found http://192.166.52.3:80/code.cfg 200
[*] Using code '404' as not found for files with extension .class
[*] Using code '404' as not found for files with extension .copy
[*] Using code '404' as not found for files with extension .conf
[*] Using code '404' as not found for files with extension .exe
[*] Using code '404' as not found for files with extension .html
[+] Found http://192.166.52.3:80/index.html 200
[*] Using code '404' as not found for files with extension .htm
[*] Using code '404' as not found for files with extension .ini
[*] Using code '404' as not found for files with extension .log
[*] Using code '404' as not found for files with extension .old
[*] Using code '404' as not found for files with extension .orig
[*] Using code '404' as not found for files with extension .php
[+] Found http://192.166.52.3:80/test.php 200
[*] Using code '404' as not found for files with extension .tar
[*] Using code '404' as not found for files with extension .tar.gz
[*] Using code '404' as not found for files with extension .tgz
[*] Using code '404' as not found for files with extension .tmp
[*] Using code '404' as not found for files with extension .temp
[*] Using code '404' as not found for files with extension .txt
[*] Using code '404' as not found for files with extension .zip
[*] Using code '404' as not found for files with extension ~
[*] Using code '404' as not found for files with extension 
[+] Found http://192.166.52.3:80/cgi-bin 301
[+] Found http://192.166.52.3:80/data 301
[+] Found http://192.166.52.3:80/doc 301
[+] Found http://192.166.52.3:80/downloads 301
[+] Found http://192.166.52.3:80/manual 301
[+] Found http://192.166.52.3:80/secure 401
[+] Found http://192.166.52.3:80/uploads 301
[+] Found http://192.166.52.3:80/users 301
[+] Found http://192.166.52.3:80/webadmin 301
[+] Found http://192.166.52.3:80/view 301
[+] Found http://192.166.52.3:80/webdav 401
[+] Found http://192.166.52.3:80/webmail 301
[+] Found http://192.166.52.3:80/~admin 403
[+] Found http://192.166.52.3:80/~bin 403
[+] Found http://192.166.52.3:80/~mail 403
[+] Found http://192.166.52.3:80/~sys 403
[*] Using code '404' as not found for files with extension 
[+] Found http://192.166.52.3:80/cgi-bin 301
[+] Found http://192.166.52.3:80/data 301
[+] Found http://192.166.52.3:80/doc 301
[+] Found http://192.166.52.3:80/downloads 301
[+] Found http://192.166.52.3:80/manual 301
[+] Found http://192.166.52.3:80/secure 401
[+] Found http://192.166.52.3:80/uploads 301
[+] Found http://192.166.52.3:80/users 301
[+] Found http://192.166.52.3:80/webadmin 301
[+] Found http://192.166.52.3:80/webdav 401
[+] Found http://192.166.52.3:80/view 301
[+] Found http://192.166.52.3:80/webmail 301
[+] Found http://192.166.52.3:80/~admin 403
[+] Found http://192.166.52.3:80/~bin 403
[+] Found http://192.166.52.3:80/~mail 403
[+] Found http://192.166.52.3:80/~sys 403
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/http/files_dir) > 
```

trying to brute force

```
msf5 auxiliary(scanner/http/http_login) > options

Module options (auxiliary/scanner/http/http_login):

   Name              Current Setting                                                           Required  Description
   ----              ---------------                                                           --------  -----------
   AUTH_URI                                                                                    no        The URI to authenticate against (default:auto)
   BLANK_PASSWORDS   false                                                                     no        Try blank passwords for all users
   BRUTEFORCE_SPEED  5                                                                         yes       How fast to bruteforce, from 0 to 5
   DB_ALL_CREDS      false                                                                     no        Try each user/password couple stored in the current database
   DB_ALL_PASS       false                                                                     no        Add all passwords in the current database to the list
   DB_ALL_USERS      false                                                                     no        Add all users in the current database to the list
   PASS_FILE         /usr/share/metasploit-framework/data/wordlists/http_default_pass.txt      no        File containing passwords, one per line
   Proxies                                                                                     no        A proxy chain of format type:host:port[,type:host:port][...]
   REQUESTTYPE       GET                                                                       no        Use HTTP-GET or HTTP-PUT for Digest-Auth, PROPFIND for WebDAV (default:GET)
   RHOSTS            192.166.52.3                                                              yes       The target address range or CIDR identifier
   RPORT             80                                                                        yes       The target port (TCP)
   SSL               false                                                                     no        Negotiate SSL/TLS for outgoing connections
   STOP_ON_SUCCESS   false                                                                     yes       Stop guessing when a credential works for a host
   THREADS           1                                                                         yes       The number of concurrent threads
   USERPASS_FILE     /usr/share/metasploit-framework/data/wordlists/http_default_userpass.txt  no        File containing users and passwords separated by space, one pair per line
   USER_AS_PASS      false                                                                     no        Try the username as the password for all users
   USER_FILE         /usr/share/metasploit-framework/data/wordlists/http_default_users.txt     no        File containing users, one per line
   VERBOSE           true                                                                      yes       Whether to print output for all attempts
   VHOST                                                                                       no        HTTP server virtual host

msf5 auxiliary(scanner/http/http_login) > set auth_uri /secure/
auth_uri => /secure/
msf5 auxiliary(scanner/http/http_login) > run

[*] Attempting to login to http://192.166.52.3:80/secure/
[-] 192.166.52.3:80 - Failed: 'admin:admin'
[-] 192.166.52.3:80 - Failed: 'admin:password'
[-] 192.166.52.3:80 - Failed: 'admin:manager'
[-] 192.166.52.3:80 - Failed: 'admin:letmein'
[-] 192.166.52.3:80 - Failed: 'admin:cisco'
[-] 192.166.52.3:80 - Failed: 'admin:default'
[-] 192.166.52.3:80 - Failed: 'admin:root'
msf5 auxiliary(scanner/http/http_login) > 
```

```
msf5 auxiliary(scanner/http/http_login) > use auxiliary/scanner/http/apache_userdir_enum 
msf5 auxiliary(scanner/http/apache_userdir_enum) > options

Module options (auxiliary/scanner/http/apache_userdir_enum):

   Name              Current Setting                                                Required  Description
   ----              ---------------                                                --------  -----------
   BRUTEFORCE_SPEED  5                                                              yes       How fast to bruteforce, from 0 to 5
   DB_ALL_CREDS      false                                                          no        Try each user/password couple stored in the current database
   DB_ALL_PASS       false                                                          no        Add all passwords in the current database to the list
   DB_ALL_USERS      false                                                          no        Add all users in the current database to the list
   Proxies                                                                          no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS            192.166.52.3                                                   yes       The target address range or CIDR identifier
   RPORT             80                                                             yes       The target port (TCP)
   SSL               false                                                          no        Negotiate SSL/TLS for outgoing connections
   TARGETURI         /                                                              yes       The path to users Home Page
   THREADS           1                                                              yes       The number of concurrent threads
   USERNAME                                                                         no        A specific username to authenticate as
   USER_FILE         /usr/share/metasploit-framework/data/wordlists/unix_users.txt  yes       File containing users, one per line
   VERBOSE           true                                                           yes       Whether to print output for all attempts
   VHOST                                                                            no        HTTP server virtual host

msf5 auxiliary(scanner/http/apache_userdir_enum) > run

[*] http://192.166.52.3/~ - Trying UserDir: ''
[*] http://192.166.52.3/ - Apache UserDir: '' not found
[*] http://192.166.52.3/~4Dgifts - Trying UserDir: '4Dgifts'
[*] http://192.166.52.3/ - Apache UserDir: '4Dgifts' not found
[*] http://192.166.52.3/~EZsetup - Trying UserDir: 'EZsetup'
[*] http://192.166.52.3/ - Apache UserDir: 'EZsetup' not found
[*] http://192.166.52.3/~OutOfBox - Trying UserDir: 'OutOfBox'
[*] http://192.166.52.3/ - Apache UserDir: 'OutOfBox' not found
[*] http://192.166.52.3/~ROOT - Trying UserDir: 'ROOT'
[*] http://192.166.52.3/ - Apache UserDir: 'ROOT' not found
[*] http://192.166.52.3/~adm - Trying UserDir: 'adm'
[*] http://192.166.52.3/ - Apache UserDir: 'adm' not found
[*] http://192.166.52.3/~admin - Trying UserDir: 'admin'
[+] http://192.166.52.3/ - Apache UserDir: 'admin' found 
[*] http://192.166.52.3/~administrator - Trying UserDir: 'administrator'
[*] http://192.166.52.3/ - Apache UserDir: 'administrator' not found
[*] http://192.166.52.3/~anon - Trying UserDir: 'anon'
[*] http://192.166.52.3/ - Apache UserDir: 'anon' not found
[*] http://192.166.52.3/~auditor - Trying UserDir: 'auditor'
[*] http://192.166.52.3/ - Apache UserDir: 'auditor' not found
[*] http://192.166.52.3/~avahi - Trying UserDir: 'avahi'
[*] http://192.166.52.3/ - Apache UserDir: 'avahi' not found
[*] http://192.166.52.3/~avahi-autoipd - Trying UserDir: 'avahi-autoipd'
[*] http://192.166.52.3/ - Apache UserDir: 'avahi-autoipd' not found
[*] http://192.166.52.3/~backup - Trying UserDir: 'backup'
[+] http://192.166.52.3/ - Apache UserDir: 'backup' found 
[*] http://192.166.52.3/~bbs - Trying UserDir: 'bbs'
[*] http://192.166.52.3/ - Apache UserDir: 'bbs' not found
[*] http://192.166.52.3/~bin - Trying UserDir: 'bin'
[+] http://192.166.52.3/ - Apache UserDir: 'bin' found 
[*] http://192.166.52.3/~checkfs - Trying UserDir: 'checkfs'
[*] http://192.166.52.3/ - Apache UserDir: 'checkfs' not found
[*] http://192.166.52.3/~checkfsys - Trying UserDir: 'checkfsys'
[*] http://192.166.52.3/ - Apache UserDir: 'checkfsys' not found
[*] http://192.166.52.3/~checksys - Trying UserDir: 'checksys'
[*] http://192.166.52.3/ - Apache UserDir: 'checksys' not found
[*] http://192.166.52.3/~chronos - Trying UserDir: 'chronos'
[*] http://192.166.52.3/ - Apache UserDir: 'chronos' not found
[*] http://192.166.52.3/~cmwlogin - Trying UserDir: 'cmwlogin'
[*] http://192.166.52.3/ - Apache UserDir: 'cmwlogin' not found
[*] http://192.166.52.3/~couchdb - Trying UserDir: 'couchdb'
[*] http://192.166.52.3/ - Apache UserDir: 'couchdb' not found
[*] http://192.166.52.3/~daemon - Trying UserDir: 'daemon'
[+] http://192.166.52.3/ - Apache UserDir: 'daemon' found 
[*] http://192.166.52.3/~dbadmin - Trying UserDir: 'dbadmin'
[+] http://192.166.52.3/ - Apache UserDir: 'dbadmin' found 
[*] http://192.166.52.3/~demo - Trying UserDir: 'demo'
[*] http://192.166.52.3/ - Apache UserDir: 'demo' not found
[*] http://192.166.52.3/~demos - Trying UserDir: 'demos'
[*] http://192.166.52.3/ - Apache UserDir: 'demos' not found
[*] http://192.166.52.3/~diag - Trying UserDir: 'diag'
[*] http://192.166.52.3/ - Apache UserDir: 'diag' not found
[*] http://192.166.52.3/~distccd - Trying UserDir: 'distccd'
[*] http://192.166.52.3/ - Apache UserDir: 'distccd' not found
[*] http://192.166.52.3/~dni - Trying UserDir: 'dni'
[*] http://192.166.52.3/ - Apache UserDir: 'dni' not found
[*] http://192.166.52.3/~fal - Trying UserDir: 'fal'
[*] http://192.166.52.3/ - Apache UserDir: 'fal' not found
[*] http://192.166.52.3/~fax - Trying UserDir: 'fax'
[*] http://192.166.52.3/ - Apache UserDir: 'fax' not found
[*] http://192.166.52.3/~ftp - Trying UserDir: 'ftp'
[*] http://192.166.52.3/ - Apache UserDir: 'ftp' not found
[*] http://192.166.52.3/~games - Trying UserDir: 'games'
[+] http://192.166.52.3/ - Apache UserDir: 'games' found 
[*] http://192.166.52.3/~gdm - Trying UserDir: 'gdm'
[*] http://192.166.52.3/ - Apache UserDir: 'gdm' not found
[*] http://192.166.52.3/~gnats - Trying UserDir: 'gnats'
[+] http://192.166.52.3/ - Apache UserDir: 'gnats' found 
[*] http://192.166.52.3/~gopher - Trying UserDir: 'gopher'
[*] http://192.166.52.3/ - Apache UserDir: 'gopher' not found
[*] http://192.166.52.3/~gropher - Trying UserDir: 'gropher'
[*] http://192.166.52.3/ - Apache UserDir: 'gropher' not found
[*] http://192.166.52.3/~guest - Trying UserDir: 'guest'
[*] http://192.166.52.3/ - Apache UserDir: 'guest' not found
[*] http://192.166.52.3/~haldaemon - Trying UserDir: 'haldaemon'
[*] http://192.166.52.3/ - Apache UserDir: 'haldaemon' not found
[*] http://192.166.52.3/~halt - Trying UserDir: 'halt'
[*] http://192.166.52.3/ - Apache UserDir: 'halt' not found
[*] http://192.166.52.3/~hplip - Trying UserDir: 'hplip'
[*] http://192.166.52.3/ - Apache UserDir: 'hplip' not found
[*] http://192.166.52.3/~informix - Trying UserDir: 'informix'
[*] http://192.166.52.3/ - Apache UserDir: 'informix' not found
[*] http://192.166.52.3/~install - Trying UserDir: 'install'
[*] http://192.166.52.3/ - Apache UserDir: 'install' not found
[*] http://192.166.52.3/~irc - Trying UserDir: 'irc'
[+] http://192.166.52.3/ - Apache UserDir: 'irc' found 
[*] http://192.166.52.3/~karaf - Trying UserDir: 'karaf'
[*] http://192.166.52.3/ - Apache UserDir: 'karaf' not found
[*] http://192.166.52.3/~kernoops - Trying UserDir: 'kernoops'
[*] http://192.166.52.3/ - Apache UserDir: 'kernoops' not found
[*] http://192.166.52.3/~libuuid - Trying UserDir: 'libuuid'
[*] http://192.166.52.3/ - Apache UserDir: 'libuuid' not found
[*] http://192.166.52.3/~list - Trying UserDir: 'list'
[+] http://192.166.52.3/ - Apache UserDir: 'list' found 
[*] http://192.166.52.3/~listen - Trying UserDir: 'listen'
[*] http://192.166.52.3/ - Apache UserDir: 'listen' not found
[*] http://192.166.52.3/~lp - Trying UserDir: 'lp'
[+] http://192.166.52.3/ - Apache UserDir: 'lp' found 
[*] http://192.166.52.3/~lpadm - Trying UserDir: 'lpadm'
[*] http://192.166.52.3/ - Apache UserDir: 'lpadm' not found
[*] http://192.166.52.3/~lpadmin - Trying UserDir: 'lpadmin'
[*] http://192.166.52.3/ - Apache UserDir: 'lpadmin' not found
[*] http://192.166.52.3/~lynx - Trying UserDir: 'lynx'
[*] http://192.166.52.3/ - Apache UserDir: 'lynx' not found
[*] http://192.166.52.3/~mail - Trying UserDir: 'mail'
[+] http://192.166.52.3/ - Apache UserDir: 'mail' found 
[*] http://192.166.52.3/~man - Trying UserDir: 'man'
[+] http://192.166.52.3/ - Apache UserDir: 'man' found 
[*] http://192.166.52.3/~me - Trying UserDir: 'me'
[*] http://192.166.52.3/ - Apache UserDir: 'me' not found
[*] http://192.166.52.3/~messagebus - Trying UserDir: 'messagebus'
[*] http://192.166.52.3/ - Apache UserDir: 'messagebus' not found
[*] http://192.166.52.3/~mountfs - Trying UserDir: 'mountfs'
[*] http://192.166.52.3/ - Apache UserDir: 'mountfs' not found
[*] http://192.166.52.3/~mountfsys - Trying UserDir: 'mountfsys'
[*] http://192.166.52.3/ - Apache UserDir: 'mountfsys' not found
[*] http://192.166.52.3/~mountsys - Trying UserDir: 'mountsys'
[*] http://192.166.52.3/ - Apache UserDir: 'mountsys' not found
[*] http://192.166.52.3/~news - Trying UserDir: 'news'
[+] http://192.166.52.3/ - Apache UserDir: 'news' found 
[*] http://192.166.52.3/~noaccess - Trying UserDir: 'noaccess'
[*] http://192.166.52.3/ - Apache UserDir: 'noaccess' not found
[*] http://192.166.52.3/~nobody - Trying UserDir: 'nobody'
[+] http://192.166.52.3/ - Apache UserDir: 'nobody' found 
[*] http://192.166.52.3/~nobody4 - Trying UserDir: 'nobody4'
[*] http://192.166.52.3/ - Apache UserDir: 'nobody4' not found
[*] http://192.166.52.3/~nuucp - Trying UserDir: 'nuucp'
[*] http://192.166.52.3/ - Apache UserDir: 'nuucp' not found
[*] http://192.166.52.3/~nxpgsql - Trying UserDir: 'nxpgsql'
[*] http://192.166.52.3/ - Apache UserDir: 'nxpgsql' not found
[*] http://192.166.52.3/~operator - Trying UserDir: 'operator'
[*] http://192.166.52.3/ - Apache UserDir: 'operator' not found
[*] http://192.166.52.3/~oracle - Trying UserDir: 'oracle'
[*] http://192.166.52.3/ - Apache UserDir: 'oracle' not found
[*] http://192.166.52.3/~pi - Trying UserDir: 'pi'
[*] http://192.166.52.3/ - Apache UserDir: 'pi' not found
[*] http://192.166.52.3/~popr - Trying UserDir: 'popr'
[*] http://192.166.52.3/ - Apache UserDir: 'popr' not found
[*] http://192.166.52.3/~postgres - Trying UserDir: 'postgres'
[*] http://192.166.52.3/ - Apache UserDir: 'postgres' not found
[*] http://192.166.52.3/~postmaster - Trying UserDir: 'postmaster'
[*] http://192.166.52.3/ - Apache UserDir: 'postmaster' not found
[*] http://192.166.52.3/~printer - Trying UserDir: 'printer'
[*] http://192.166.52.3/ - Apache UserDir: 'printer' not found
[*] http://192.166.52.3/~proxy - Trying UserDir: 'proxy'
[+] http://192.166.52.3/ - Apache UserDir: 'proxy' found 
[*] http://192.166.52.3/~pulse - Trying UserDir: 'pulse'
[*] http://192.166.52.3/ - Apache UserDir: 'pulse' not found
[*] http://192.166.52.3/~rfindd - Trying UserDir: 'rfindd'
[*] http://192.166.52.3/ - Apache UserDir: 'rfindd' not found
[*] http://192.166.52.3/~rje - Trying UserDir: 'rje'
[*] http://192.166.52.3/ - Apache UserDir: 'rje' not found
[*] http://192.166.52.3/~root - Trying UserDir: 'root'
[*] http://192.166.52.3/ - Apache UserDir: 'root' not found
[*] http://192.166.52.3/~rooty - Trying UserDir: 'rooty'
[+] http://192.166.52.3/ - Apache UserDir: 'rooty' found 
[*] http://192.166.52.3/~saned - Trying UserDir: 'saned'
[*] http://192.166.52.3/ - Apache UserDir: 'saned' not found
[*] http://192.166.52.3/~service - Trying UserDir: 'service'
[*] http://192.166.52.3/ - Apache UserDir: 'service' not found
[*] http://192.166.52.3/~setup - Trying UserDir: 'setup'
[*] http://192.166.52.3/ - Apache UserDir: 'setup' not found
[*] http://192.166.52.3/~sgiweb - Trying UserDir: 'sgiweb'
[*] http://192.166.52.3/ - Apache UserDir: 'sgiweb' not found
[*] http://192.166.52.3/~sigver - Trying UserDir: 'sigver'
[*] http://192.166.52.3/ - Apache UserDir: 'sigver' not found
[*] http://192.166.52.3/~speech-dispatcher - Trying UserDir: 'speech-dispatcher'
[*] http://192.166.52.3/ - Apache UserDir: 'speech-dispatcher' not found
[*] http://192.166.52.3/~sshd - Trying UserDir: 'sshd'
[*] http://192.166.52.3/ - Apache UserDir: 'sshd' not found
[*] http://192.166.52.3/~sym - Trying UserDir: 'sym'
[*] http://192.166.52.3/ - Apache UserDir: 'sym' not found
[*] http://192.166.52.3/~symop - Trying UserDir: 'symop'
[*] http://192.166.52.3/ - Apache UserDir: 'symop' not found
[*] http://192.166.52.3/~sync - Trying UserDir: 'sync'
[+] http://192.166.52.3/ - Apache UserDir: 'sync' found 
[*] http://192.166.52.3/~sys - Trying UserDir: 'sys'
[+] http://192.166.52.3/ - Apache UserDir: 'sys' found 
[*] http://192.166.52.3/~sysadm - Trying UserDir: 'sysadm'
[*] http://192.166.52.3/ - Apache UserDir: 'sysadm' not found
[*] http://192.166.52.3/~sysadmin - Trying UserDir: 'sysadmin'
[*] http://192.166.52.3/ - Apache UserDir: 'sysadmin' not found
[*] http://192.166.52.3/~sysbin - Trying UserDir: 'sysbin'
[*] http://192.166.52.3/ - Apache UserDir: 'sysbin' not found
[*] http://192.166.52.3/~syslog - Trying UserDir: 'syslog'
[*] http://192.166.52.3/ - Apache UserDir: 'syslog' not found
[*] http://192.166.52.3/~system_admin - Trying UserDir: 'system_admin'
[*] http://192.166.52.3/ - Apache UserDir: 'system_admin' not found
[*] http://192.166.52.3/~trouble - Trying UserDir: 'trouble'
[*] http://192.166.52.3/ - Apache UserDir: 'trouble' not found
[*] http://192.166.52.3/~udadmin - Trying UserDir: 'udadmin'
[*] http://192.166.52.3/ - Apache UserDir: 'udadmin' not found
[*] http://192.166.52.3/~ultra - Trying UserDir: 'ultra'
[+] http://192.166.52.3/ - Apache UserDir: 'ultra' found 
[*] http://192.166.52.3/~umountfs - Trying UserDir: 'umountfs'
[*] http://192.166.52.3/ - Apache UserDir: 'umountfs' not found
[*] http://192.166.52.3/~umountfsys - Trying UserDir: 'umountfsys'
[*] http://192.166.52.3/ - Apache UserDir: 'umountfsys' not found
[*] http://192.166.52.3/~umountsys - Trying UserDir: 'umountsys'
[*] http://192.166.52.3/ - Apache UserDir: 'umountsys' not found
[*] http://192.166.52.3/~unix - Trying UserDir: 'unix'
[*] http://192.166.52.3/ - Apache UserDir: 'unix' not found
[*] http://192.166.52.3/~us_admin - Trying UserDir: 'us_admin'
[*] http://192.166.52.3/ - Apache UserDir: 'us_admin' not found
[*] http://192.166.52.3/~user - Trying UserDir: 'user'
[*] http://192.166.52.3/ - Apache UserDir: 'user' not found
[*] http://192.166.52.3/~uucp - Trying UserDir: 'uucp'
[+] http://192.166.52.3/ - Apache UserDir: 'uucp' found 
[*] http://192.166.52.3/~uucpadm - Trying UserDir: 'uucpadm'
[*] http://192.166.52.3/ - Apache UserDir: 'uucpadm' not found
[*] http://192.166.52.3/~web - Trying UserDir: 'web'
[*] http://192.166.52.3/ - Apache UserDir: 'web' not found
[*] http://192.166.52.3/~webmaster - Trying UserDir: 'webmaster'
[*] http://192.166.52.3/ - Apache UserDir: 'webmaster' not found
[*] http://192.166.52.3/~www - Trying UserDir: 'www'
[*] http://192.166.52.3/ - Apache UserDir: 'www' not found
[*] http://192.166.52.3/~www-data - Trying UserDir: 'www-data'
[*] http://192.166.52.3/ - Apache UserDir: 'www-data' not found
[*] http://192.166.52.3/~xpdb - Trying UserDir: 'xpdb'
[*] http://192.166.52.3/ - Apache UserDir: 'xpdb' not found
[*] http://192.166.52.3/~xpopr - Trying UserDir: 'xpopr'
[*] http://192.166.52.3/ - Apache UserDir: 'xpopr' not found
[*] http://192.166.52.3/~zabbix - Trying UserDir: 'zabbix'
[*] http://192.166.52.3/ - Apache UserDir: 'zabbix' not found
[*] http://192.166.52.3/~vagrant - Trying UserDir: 'vagrant'
[*] http://192.166.52.3/ - Apache UserDir: 'vagrant' not found
[+] http://192.166.52.3/ - Users found: admin, backup, bin, daemon, dbadmin, games, gnats, irc, list, lp, mail, man, news, nobody, proxy, rooty, sync, sys, ultra, uucp
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/http/apache_userdir_enum) >
```

```
msf5 auxiliary(scanner/http/apache_userdir_enum) > use auxiliary/scanner/http/http_login
msf5 auxiliary(scanner/http/http_login) >  echo "rooty" > users.txt
[*] exec: echo "rooty" > users.txt

msf5 auxiliary(scanner/http/http_login) > set user_file /root/users.txt
user_file => /root/users.txt
msf5 auxiliary(scanner/http/http_login) > set pass_file /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
pass_file => /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
msf5 auxiliary(scanner/http/http_login) > unset USERPASS_FILE 
Unsetting USERPASS_FILE...
msf5 auxiliary(scanner/http/http_login) > options

Module options (auxiliary/scanner/http/http_login):

   Name              Current Setting                                                           Required  Description
   ----              ---------------                                                           --------  -----------
   AUTH_URI          /secure/                                                                  no        The URI to authenticate against (default:auto)
   BLANK_PASSWORDS   false                                                                     no        Try blank passwords for all users
   BRUTEFORCE_SPEED  5                                                                         yes       How fast to bruteforce, from 0 to 5
   DB_ALL_CREDS      false                                                                     no        Try each user/password couple stored in the current database
   DB_ALL_PASS       false                                                                     no        Add all passwords in the current database to the list
   DB_ALL_USERS      false                                                                     no        Add all users in the current database to the list
   PASS_FILE         /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt         no        File containing passwords, one per line
   Proxies                                                                                     no        A proxy chain of format type:host:port[,type:host:port][...]
   REQUESTTYPE       GET                                                                       no        Use HTTP-GET or HTTP-PUT for Digest-Auth, PROPFIND for WebDAV (default:GET)
   RHOSTS            192.166.52.3                                                              yes       The target address range or CIDR identifier
   RPORT             80                                                                        yes       The target port (TCP)
   SSL               false                                                                     no        Negotiate SSL/TLS for outgoing connections
   STOP_ON_SUCCESS   false                                                                     yes       Stop guessing when a credential works for a host
   THREADS           1                                                                         yes       The number of concurrent threads
   USERPASS_FILE     /usr/share/metasploit-framework/data/wordlists/http_default_userpass.txt  no        File containing users and passwords separated by space, one pair per line
   USER_AS_PASS      false                                                                     no        Try the username as the password for all users
   USER_FILE         /root/users.txt                                                           no        File containing users, one per line
   VERBOSE           true                                                                      yes       Whether to print output for all attempts
   VHOST                                                                                       no        HTTP server virtual host

msf5 auxiliary(scanner/http/http_login) > run

[*] Attempting to login to http://192.166.52.3:80/secure/
[-] 192.166.52.3:80 - Failed: 'rooty:admin'
[-] 192.166.52.3:80 - Failed: 'rooty:123456'
[-] 192.166.52.3:80 - Failed: 'rooty:12345'
[-] 192.166.52.3:80 - Failed: 'rooty:123456789'
[-] 192.166.52.3:80 - Failed: 'rooty:password'
[-] 192.166.52.3:80 - Failed: 'rooty:iloveyou'
[-] 192.166.52.3:80 - Failed: 'rooty:princess'
[-] 192.166.52.3:80 - Failed: 'rooty:1234567'
[-] 192.166.52.3:80 - Failed: 'rooty:12345678'
[-] 192.166.52.3:80 - Failed: 'rooty:abc123'
[-] 192.166.52.3:80 - Failed: 'rooty:nicole'
[-] 192.166.52.3:80 - Failed: 'rooty:daniel'
[-] 192.166.52.3:80 - Failed: 'rooty:babygirl'
[-] 192.166.52.3:80 - Failed: 'rooty:monkey'
[-] 192.166.52.3:80 - Failed: 'rooty:lovely'
[-] 192.166.52.3:80 - Failed: 'rooty:jessica'
[-] 192.166.52.3:80 - Failed: 'rooty:654321'
[-] 192.166.52.3:80 - Failed: 'rooty:michael'
[-] 192.166.52.3:80 - Failed: 'rooty:ashley'
[-] 192.166.52.3:80 - Failed: 'rooty:qwerty'
[-] 192.166.52.3:80 - Failed: 'rooty:111111'
[-] 192.166.52.3:80 - Failed: 'rooty:iloveu'
[-] 192.166.52.3:80 - Failed: 'rooty:000000'
[-] 192.166.52.3:80 - Failed: 'rooty:michelle'
[-] 192.166.52.3:80 - Failed: 'rooty:tigger'
[-] 192.166.52.3:80 - Failed: 'rooty:sunshine'
[-] 192.166.52.3:80 - Failed: 'rooty:chocolate'
[-] 192.166.52.3:80 - Failed: 'rooty:password1'
[-] 192.166.52.3:80 - Failed: 'rooty:soccer'
[-] 192.166.52.3:80 - Failed: 'rooty:anthony'
[-] 192.166.52.3:80 - Failed: 'rooty:friends'
[-] 192.166.52.3:80 - Failed: 'rooty:butterfly'
[-] 192.166.52.3:80 - Failed: 'rooty:purple'
[-] 192.166.52.3:80 - Failed: 'rooty:angel'
[-] 192.166.52.3:80 - Failed: 'rooty:jordan'
[-] 192.166.52.3:80 - Failed: 'rooty:liverpool'
[-] 192.166.52.3:80 - Failed: 'rooty:justin'
[-] 192.166.52.3:80 - Failed: 'rooty:loveme'
[-] 192.166.52.3:80 - Failed: 'rooty:fuckyou'
[-] 192.166.52.3:80 - Failed: 'rooty:123123'
[-] 192.166.52.3:80 - Failed: 'rooty:football'
[-] 192.166.52.3:80 - Failed: 'rooty:secret'
[-] 192.166.52.3:80 - Failed: 'rooty:andrea'
[-] 192.166.52.3:80 - Failed: 'rooty:carlos'
[-] 192.166.52.3:80 - Failed: 'rooty:jennifer'
[-] 192.166.52.3:80 - Failed: 'rooty:joshua'
[-] 192.166.52.3:80 - Failed: 'rooty:bubbles'
[-] 192.166.52.3:80 - Failed: 'rooty:1234567890'
[-] 192.166.52.3:80 - Failed: 'rooty:superman'
[-] 192.166.52.3:80 - Failed: 'rooty:hannah'
[-] 192.166.52.3:80 - Failed: 'rooty:amanda'
[-] 192.166.52.3:80 - Failed: 'rooty:loveyou'
[-] 192.166.52.3:80 - Failed: 'rooty:pretty'
[-] 192.166.52.3:80 - Failed: 'rooty:basketball'
[-] 192.166.52.3:80 - Failed: 'rooty:andrew'
[-] 192.166.52.3:80 - Failed: 'rooty:angels'
[-] 192.166.52.3:80 - Failed: 'rooty:tweety'
[-] 192.166.52.3:80 - Failed: 'rooty:flower'
[-] 192.166.52.3:80 - Failed: 'rooty:playboy'
[-] 192.166.52.3:80 - Failed: 'rooty:hello'
[-] 192.166.52.3:80 - Failed: 'rooty:elizabeth'
[-] 192.166.52.3:80 - Failed: 'rooty:hottie'
[-] 192.166.52.3:80 - Failed: 'rooty:tinkerbell'
[-] 192.166.52.3:80 - Failed: 'rooty:charlie'
[-] 192.166.52.3:80 - Failed: 'rooty:samantha'
[-] 192.166.52.3:80 - Failed: 'rooty:barbie'
[-] 192.166.52.3:80 - Failed: 'rooty:chelsea'
[-] 192.166.52.3:80 - Failed: 'rooty:lovers'
[-] 192.166.52.3:80 - Failed: 'rooty:teamo'
[-] 192.166.52.3:80 - Failed: 'rooty:jasmine'
[-] 192.166.52.3:80 - Failed: 'rooty:brandon'
[-] 192.166.52.3:80 - Failed: 'rooty:666666'
[-] 192.166.52.3:80 - Failed: 'rooty:shadow'
[-] 192.166.52.3:80 - Failed: 'rooty:melissa'
[-] 192.166.52.3:80 - Failed: 'rooty:eminem'
[-] 192.166.52.3:80 - Failed: 'rooty:matthew'
[-] 192.166.52.3:80 - Failed: 'rooty:robert'
[-] 192.166.52.3:80 - Failed: 'rooty:danielle'
[-] 192.166.52.3:80 - Failed: 'rooty:forever'
[-] 192.166.52.3:80 - Failed: 'rooty:family'
[-] 192.166.52.3:80 - Failed: 'rooty:jonathan'
[-] 192.166.52.3:80 - Failed: 'rooty:987654321'
[-] 192.166.52.3:80 - Failed: 'rooty:computer'
[-] 192.166.52.3:80 - Failed: 'rooty:whatever'
[-] 192.166.52.3:80 - Failed: 'rooty:dragon'
[-] 192.166.52.3:80 - Failed: 'rooty:vanessa'
[-] 192.166.52.3:80 - Failed: 'rooty:cookie'
[-] 192.166.52.3:80 - Failed: 'rooty:naruto'
[-] 192.166.52.3:80 - Failed: 'rooty:summer'
[-] 192.166.52.3:80 - Failed: 'rooty:sweety'
```

