### http3

**Questions**

1. Which web server software is running on the target server and also find out the version using nmap.
2. Which web server software is running on the target server and also find out the version using suitable metasploit module.
3. Check what web app is hosted on the web server using curl command.
4. Check what web app is hosted on the web server using wget command.
5. Check what web app is hosted on the web server using browsh CLI based browser.
6. Check what web app is hosted on the web server using lynx CLI based browser.
7. Perform bruteforce on webserver directories and list the names of directories found. Use brute_dirs metasploit module.
8. Use the directory buster (dirb) with tool/usr/share/metasploit-framework/data/wordlists/directory.txt dictionary to check if any directory is present in the root folder of the web server. List the names of found directories.
9. Which bot is specifically banned from accessing a specific directory?

[[ip]]

```
root@INE:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
4959: eth0@if4960: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:06 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.6/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
4962: eth1@if4963: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:d4:6a:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.212.106.2/24 brd 192.212.106.255 scope global eth1
       valid_lft forever preferred_lft forever
```

[[ping]]

```
root@INE:~# ping -c 5 192.212.106.3
PING 192.212.106.3 (192.212.106.3) 56(84) bytes of data.
64 bytes from 192.212.106.3: icmp_seq=1 ttl=64 time=0.080 ms
64 bytes from 192.212.106.3: icmp_seq=2 ttl=64 time=0.047 ms
64 bytes from 192.212.106.3: icmp_seq=3 ttl=64 time=0.036 ms
64 bytes from 192.212.106.3: icmp_seq=4 ttl=64 time=0.034 ms
64 bytes from 192.212.106.3: icmp_seq=5 ttl=64 time=0.043 ms

--- 192.212.106.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4079ms
rtt min/avg/max/mdev = 0.034/0.048/0.080/0.016 ms
```

[[Hack2.0/NMAP#General Scan|NMAP basic scan]]

```
root@INE:~# nmap 192.212.106.3
Starting Nmap 7.92 ( https://nmap.org ) at 2023-07-10 21:17 IST
Nmap scan report for om8muy127jhgy419qt3i7fd4d.temp-network_a-212-106 (192.212.106.3)
Host is up (0.0000090s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
80/tcp open  http
MAC Address: 02:42:C0:D4:6A:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.23 seconds
```

```
root@INE:~# nmap -p80 -sV -O 192.212.106.3
Starting Nmap 7.92 ( https://nmap.org ) at 2023-07-10 21:17 IST
Nmap scan report for om8muy127jhgy419qt3i7fd4d.temp-network_a-212-106 (192.212.106.3)
Host is up (0.000059s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
MAC Address: 02:42:C0:D4:6A:03 (Unknown)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 5.X
OS CPE: cpe:/o:linux:linux_kernel:5
OS details: Linux 5.0 - 5.3
Network Distance: 1 hop

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.98 seconds
```

```
root@INE:~# nmap -p80 192.212.106.3 --script banner -sV
Starting Nmap 7.92 ( https://nmap.org ) at 2023-07-10 21:19 IST
Nmap scan report for om8muy127jhgy419qt3i7fd4d.temp-network_a-212-106 (192.212.106.3)
Host is up (0.000032s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
MAC Address: 02:42:C0:D4:6A:03 (Unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 16.53 seconds
```

[[metasploit]]

```
msf6 > search http_version

Matching Modules
================

   #  Name                                 Disclosure Date  Rank    Check  Description
   -  ----                                 ---------------  ----    -----  -----------
   0  auxiliary/scanner/http/http_version                   normal  No     HTTP Version Detection


Interact with a module by name or index. For example info 0, use 0 or use auxiliary/scanner/http/http_version

msf6 > use 0
msf6 auxiliary(scanner/http/http_version) > options

Module options (auxiliary/scanner/http/http_version):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   Proxies                   no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                    yes       The target host(s), see https://github.com/rapid7/metasploit-framework/wiki/Using-Metasploit
   RPORT    80               yes       The target port (TCP)
   SSL      false            no        Negotiate SSL/TLS for outgoing connections
   THREADS  1                yes       The number of concurrent threads (max one per host)
   VHOST                     no        HTTP server virtual host

msf6 auxiliary(scanner/http/http_version) > set rhosts 192.212.106.3
rhosts => 192.212.106.3
msf6 auxiliary(scanner/http/http_version) > run

[+] 192.212.106.3:80 Apache/2.4.18 (Ubuntu)
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

[[curl]]

```
root@INE:~# curl 192.212.106.3

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
  <!--
    Modified from the Debian original for Ubuntu
    Last updated: 2014-03-19
    See: https://launchpad.net/bugs/1288690
  -->
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <title>Apache2 Ubuntu Default Page: It works</title>
    <style type="text/css" media="screen">
  * {
    margin: 0px 0px 0px 0px;
    padding: 0px 0px 0px 0px;
  }

  body, html {
    padding: 3px 3px 3px 3px;

    background-color: #D8DBE2;

    font-family: Verdana, sans-serif;
    font-size: 11pt;
    text-align: center;
  }

  div.main_page {
    position: relative;
    display: table;

    width: 800px;

    margin-bottom: 3px;
    margin-left: auto;
    margin-right: auto;
    padding: 0px 0px 0px 0px;

    border-width: 2px;
    border-color: #212738;
    border-style: solid;

    background-color: #FFFFFF;

    text-align: center;
  }

  div.page_header {
    height: 99px;
    width: 100%;

    background-color: #F5F6F7;
  }

  div.page_header span {
    margin: 15px 0px 0px 50px;

    font-size: 180%;
    font-weight: bold;
  }

  div.page_header img {
    margin: 3px 0px 0px 40px;

    border: 0px 0px 0px;
  }

  div.table_of_contents {
    clear: left;

    min-width: 200px;

    margin: 3px 3px 3px 3px;

    background-color: #FFFFFF;

    text-align: left;
  }

  div.table_of_contents_item {
    clear: left;

    width: 100%;

    margin: 4px 0px 0px 0px;

    background-color: #FFFFFF;

    color: #000000;
    text-align: left;
  }

  div.table_of_contents_item a {
    margin: 6px 0px 0px 6px;
  }

  div.content_section {
    margin: 3px 3px 3px 3px;

    background-color: #FFFFFF;

    text-align: left;
  }

  div.content_section_text {
    padding: 4px 8px 4px 8px;

    color: #000000;
    font-size: 100%;
  }

  div.content_section_text pre {
    margin: 8px 0px 8px 0px;
    padding: 8px 8px 8px 8px;

    border-width: 1px;
    border-style: dotted;
    border-color: #000000;

    background-color: #F5F6F7;

    font-style: italic;
  }

  div.content_section_text p {
    margin-bottom: 6px;
  }

  div.content_section_text ul, div.content_section_text li {
    padding: 4px 8px 4px 16px;
  }

  div.section_header {
    padding: 3px 6px 3px 6px;

    background-color: #8E9CB2;

    color: #FFFFFF;
    font-weight: bold;
    font-size: 112%;
    text-align: center;
  }

  div.section_header_red {
    background-color: #CD214F;
  }

  div.section_header_grey {
    background-color: #9F9386;
  }

  .floating_element {
    position: relative;
    float: left;
  }

  div.table_of_contents_item a,
  div.content_section_text a {
    text-decoration: none;
    font-weight: bold;
  }

  div.table_of_contents_item a:link,
  div.table_of_contents_item a:visited,
  div.table_of_contents_item a:active {
    color: #000000;
  }

  div.table_of_contents_item a:hover {
    background-color: #000000;

    color: #FFFFFF;
  }

  div.content_section_text a:link,
  div.content_section_text a:visited,
   div.content_section_text a:active {
    background-color: #DCDFE6;

    color: #000000;
  }

  div.content_section_text a:hover {
    background-color: #000000;

    color: #DCDFE6;
  }

  div.validator {
  }
    </style>
  </head>
  <body>
    <div class="main_page">
      <div class="page_header floating_element">
        <img src="/icons/ubuntu-logo.png" alt="Ubuntu Logo" class="floating_element"/>
        <span class="floating_element">
          Apache2 Ubuntu Default Page
        </span>
      </div>
<!--      <div class="table_of_contents floating_element">
        <div class="section_header section_header_grey">
          TABLE OF CONTENTS
        </div>
        <div class="table_of_contents_item floating_element">
          <a href="#about">About</a>
        </div>
        <div class="table_of_contents_item floating_element">
          <a href="#changes">Changes</a>
        </div>
        <div class="table_of_contents_item floating_element">
          <a href="#scope">Scope</a>
        </div>
        <div class="table_of_contents_item floating_element">
          <a href="#files">Config files</a>
        </div>
      </div>
-->
      <div class="content_section floating_element">


        <div class="section_header section_header_red">
          <div id="about"></div>
          It works!
        </div>
        <div class="content_section_text">
          <p>
                This is the default welcome page used to test the correct 
                operation of the Apache2 server after installation on Ubuntu systems.
                It is based on the equivalent page on Debian, from which the Ubuntu Apache
                packaging is derived.
                If you can read this page, it means that the Apache HTTP server installed at
                this site is working properly. You should <b>replace this file</b> (located at
                <tt>/var/www/html/index.html</tt>) before continuing to operate your HTTP server.
          </p>


          <p>
                If you are a normal user of this web site and don't know what this page is
                about, this probably means that the site is currently unavailable due to
                maintenance.
                If the problem persists, please contact the site's administrator.
          </p>

        </div>
        <div class="section_header">
          <div id="changes"></div>
                Configuration Overview
        </div>
        <div class="content_section_text">
          <p>
                Ubuntu's Apache2 default configuration is different from the
                upstream default configuration, and split into several files optimized for
                interaction with Ubuntu tools. The configuration system is
                <b>fully documented in
                /usr/share/doc/apache2/README.Debian.gz</b>. Refer to this for the full
                documentation. Documentation for the web server itself can be
                found by accessing the <a href="/manual">manual</a> if the <tt>apache2-doc</tt>
                package was installed on this server.

          </p>
          <p>
                The configuration layout for an Apache2 web server installation on Ubuntu systems is as follows:
          </p>
          <pre>
/etc/apache2/
|-- apache2.conf
|       `--  ports.conf
|-- mods-enabled
|       |-- *.load
|       `-- *.conf
|-- conf-enabled
|       `-- *.conf
|-- sites-enabled
|       `-- *.conf
          </pre>
          <ul>
                        <li>
                           <tt>apache2.conf</tt> is the main configuration
                           file. It puts the pieces together by including all remaining configuration
                           files when starting up the web server.
                        </li>

                        <li>
                           <tt>ports.conf</tt> is always included from the
                           main configuration file. It is used to determine the listening ports for
                           incoming connections, and this file can be customized anytime.
                        </li>

                        <li>
                           Configuration files in the <tt>mods-enabled/</tt>,
                           <tt>conf-enabled/</tt> and <tt>sites-enabled/</tt> directories contain
                           particular configuration snippets which manage modules, global configuration
                           fragments, or virtual host configurations, respectively.
                        </li>

                        <li>
                           They are activated by symlinking available
                           configuration files from their respective
                           *-available/ counterparts. These should be managed
                           by using our helpers
                           <tt>
                                <a href="http://manpages.debian.org/cgi-bin/man.cgi?query=a2enmod">a2enmod</a>,
                                <a href="http://manpages.debian.org/cgi-bin/man.cgi?query=a2dismod">a2dismod</a>,
                           </tt>
                           <tt>
                                <a href="http://manpages.debian.org/cgi-bin/man.cgi?query=a2ensite">a2ensite</a>,
                                <a href="http://manpages.debian.org/cgi-bin/man.cgi?query=a2dissite">a2dissite</a>,
                            </tt>
                                and
                           <tt>
                                <a href="http://manpages.debian.org/cgi-bin/man.cgi?query=a2enconf">a2enconf</a>,
                                <a href="http://manpages.debian.org/cgi-bin/man.cgi?query=a2disconf">a2disconf</a>
                           </tt>. See their respective man pages for detailed information.
                        </li>

                        <li>
                           The binary is called apache2. Due to the use of
                           environment variables, in the default configuration, apache2 needs to be
                           started/stopped with <tt>/etc/init.d/apache2</tt> or <tt>apache2ctl</tt>.
                           <b>Calling <tt>/usr/bin/apache2</tt> directly will not work</b> with the
                           default configuration.
                        </li>
          </ul>
        </div>

        <div class="section_header">
            <div id="docroot"></div>
                Document Roots
        </div>

        <div class="content_section_text">
            <p>
                By default, Ubuntu does not allow access through the web browser to
                <em>any</em> file apart of those located in <tt>/var/www</tt>,
                <a href="http://httpd.apache.org/docs/2.4/mod/mod_userdir.html">public_html</a>
                directories (when enabled) and <tt>/usr/share</tt> (for web
                applications). If your site is using a web document root
                located elsewhere (such as in <tt>/srv</tt>) you may need to whitelist your
                document root directory in <tt>/etc/apache2/apache2.conf</tt>.
            </p>
            <p>
                The default Ubuntu document root is <tt>/var/www/html</tt>. You
                can make your own virtual hosts under /var/www. This is different
                to previous releases which provides better security out of the box.
            </p>
        </div>

        <div class="section_header">
          <div id="bugs"></div>
                Reporting Problems
        </div>
        <div class="content_section_text">
          <p>
                Please use the <tt>ubuntu-bug</tt> tool to report bugs in the
                Apache2 package with Ubuntu. However, check <a
                href="https://bugs.launchpad.net/ubuntu/+source/apache2">existing
                bug reports</a> before reporting a new bug.
          </p>
          <p>
                Please report bugs specific to modules (such as PHP and others)
                to respective packages, not to the web server itself.
          </p>
        </div>




      </div>
    </div>
    <div class="validator">
    </div>
  </body>
</html>
```

[[wget]]

```
root@INE:~# wget "http://192.212.106.3"
--2023-07-10 21:31:07--  http://192.212.106.3/
Connecting to 192.212.106.3:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 11321 (11K) [text/html]
Saving to: ‘index.html’

index.html                                                          100%[================================================================================================================================================================>]  11.06K  --.-KB/s    in 0s      

2023-07-10 21:31:07 (201 MB/s) - ‘index.html’ saved [11321/11321]
```

[[Hack2.0/browsh|browsh]]

```
browsh --startup-url http://192.212.106.3
```

![[Pasted image 20230710213308.png]]

[[Hack2.0/lynx|lynx]]

```
lynx http://192.212.106.3
```

```
                                                                                                                                             Apache2 Ubuntu Default Page: It works (p1 of 2)
   Ubuntu Logo Apache2 Ubuntu Default Page                                                                                                                                                  
   It works!                                                                                                                                                                                
                                                                                                                                                                                            
   This is the default welcome page used to test the correct operation of the Apache2 server after installation on Ubuntu systems. It is based on the equivalent page on Debian, from       
   which the Ubuntu Apache packaging is derived. If you can read this page, it means that the Apache HTTP server installed at this site is working properly. You should replace this        
   file (located at /var/www/html/index.html) before continuing to operate your HTTP server.                                                                                                
                                                                                                                                                                                            
   If you are a normal user of this web site and don't know what this page is about, this probably means that the site is currently unavailable due to maintenance. If the problem          
   persists, please contact the site's administrator.                                                                                                                                       
   Configuration Overview                                                                                                                                                                   
                                                                                                                                                                                            
   Ubuntu's Apache2 default configuration is different from the upstream default configuration, and split into several files optimized for interaction with Ubuntu tools. The               
   configuration system is fully documented in /usr/share/doc/apache2/README.Debian.gz. Refer to this for the full documentation. Documentation for the web server itself can be found      
   by accessing the manual if the apache2-doc package was installed on this server.                                                                                                         
                                                                                                                                                                                            
   The configuration layout for an Apache2 web server installation on Ubuntu systems is as follows:                                                                                         
/etc/apache2/                                                                                                                                                                               
|-- apache2.conf                                                                                                                                                                            
|       `--  ports.conf                                                                                                                                                                     
|-- mods-enabled                                                                                                                                                                            
|       |-- *.load                                                                                                                                                                          
|       `-- *.conf                                                                                                                                                                          
|-- conf-enabled                                                                                                                                                                            
|       `-- *.conf                                                                                                                                                                          
|-- sites-enabled                                                                                                                                                                           
|       `-- *.conf
```

[[metasploit]]

```
msf6 > search brute_dirs

Matching Modules
================

   #  Name                               Disclosure Date  Rank    Check  Description
   -  ----                               ---------------  ----    -----  -----------
   0  auxiliary/scanner/http/brute_dirs                   normal  No     HTTP Directory Brute Force Scanner


Interact with a module by name or index. For example info 0, use 0 or use auxiliary/scanner/http/brute_dirs

msf6 > use 0
msf6 auxiliary(scanner/http/brute_dirs) > options

Module options (auxiliary/scanner/http/brute_dirs):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   DELAY    0                yes       The delay between connections, per thread, in milliseconds                                                                                           
   FORMAT   a,aa,aaa         yes       The expected directory format (a alpha, d digit, A upperalpha)                                                                                       
   JITTER   0                yes       The delay jitter factor (maximum value by which to +/- DELAY) in milliseconds.
   PATH     /                yes       The path to identify directories
   Proxies                   no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                    yes       The target host(s), see https://github.com/rapid7/metasploit-framework/wiki/Using-Metasploit
   RPORT    80               yes       The target port (TCP)
   SSL      false            no        Negotiate SSL/TLS for outgoing connections
   THREADS  1                yes       The number of concurrent threads (max one per host)
   TIMEOUT  20               yes       The socket connect/read timeout in seconds
   VHOST                     no        HTTP server virtual host

msf6 auxiliary(scanner/http/brute_dirs) > set rhosts 192.212.106.3
rhosts => 192.212.106.3                                                                                                                                                                     
msf6 auxiliary(scanner/http/brute_dirs) > run                                                                                                                                               
                                                                                                                                                                                            
[*] Using code '404' as not found.                                                                                                                                                          
[+] Found http://192.212.106.3:80/dir/ 200                                                                                                                                                                       
[+] Found http://192.212.106.3:80/src/ 200                                                                                                                                                                       
[*] Scanned 1 of 1 hosts (100% complete)                                                                                                                                                                         
[*] Auxiliary module execution completed                                                                                                                                                                                                   
msf6 auxiliary(scanner/http/brute_dirs) >   
```

[[Hack2.0/Commands2.0/dirb|dirb]]

```
root@INE:~# dirb http://192.212.106.3 /usr/share/metasploit-framework/data/wordlists/directory.txt 

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Mon Jul 10 21:40:54 2023
URL_BASE: http://192.212.106.3/
WORDLIST_FILES: /usr/share/metasploit-framework/data/wordlists/directory.txt

-----------------

GENERATED WORDS: 24                                                            

---- Scanning URL: http://192.212.106.3/ ----
+ http://192.212.106.3//data (CODE:301|SIZE:313)                                                                                                                                                                                          
+ http://192.212.106.3//dir (CODE:301|SIZE:312)                                                                                                                                                                                                                                                                                                                                                                                                                
-----------------
END_TIME: Mon Jul 10 21:40:54 2023
DOWNLOADED: 24 - FOUND: 2
```

[[metasploit]]

```
root@INE:~# msfconsole
                                                  

                                   .,,.                  .
                                .\$$$$$L..,,==aaccaacc%#s$b.       d8,    d8P
                     d8P        #$$$$$$$$$$$$$$$$$$$$$$$$$$$b.    `BP  d888888p
                  d888888P      '7$$$$\""""''^^`` .7$$$|D*"'```         ?88'
  d8bd8b.d8p d8888b ?88' d888b8b            _.os#$|8*"`   d8P       ?8b  88P
  88P`?P'?P d8b_,dP 88P d8P' ?88       .oaS###S*"`       d8P d8888b $whi?88b 88b
 d88  d8 ?8 88b     88b 88b  ,88b .osS$$$$*" ?88,.d88b, d88 d8P' ?88 88P `?8b
d88' d88b 8b`?8888P'`?8b`?88P'.aS$$$$Q*"`    `?88'  ?88 ?88 88b  d88 d88
                          .a#$$$$$$"`          88b  d8P  88b`?8888P'
                       ,s$$$$$$$"`             888888P'   88n      _.,,,ass;:
                    .a$$$$$$$P`               d88P'    .,.ass%#S$$$$$$$$$$$$$$'
                 .a$###$$$P`           _.,,-aqsc#SS$$$$$$$$$$$$$$$$$$$$$$$$$$'
              ,a$$###$$P`  _.,-ass#S$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$####SSSS'
           .a$$$$$$$$$$SSS$$$$$$$$$$$$$$$$$$$$$$$$$$$$SS##==--""''^^/$$$$$$'
_______________________________________________________________   ,&$$$$$$'_____
                                                                 ll&&$$$$'
                                                              .;;lll&&&&'
                                                            ...;;lllll&'
                                                          ......;;;llll;;;....
                                                           ` ......;;;;... .  .


       =[ metasploit v6.1.24-dev                          ]
+ -- --=[ 2193 exploits - 1161 auxiliary - 400 post       ]
+ -- --=[ 600 payloads - 45 encoders - 10 nops            ]
+ -- --=[ 9 evasion                                       ]

Metasploit tip: Tired of setting RHOSTS for modules? Try 
globally setting it with setg RHOSTS x.x.x.x

msf6 > search robots

Matching Modules
================

   #  Name                               Disclosure Date  Rank    Check  Description
   -  ----                               ---------------  ----    -----  -----------
   0  auxiliary/scanner/http/robots_txt                   normal  No     HTTP Robots.txt Content Scanner                                                                                    
                                                                                                                                                                                            
                                                                                                                                                                                            
Interact with a module by name or index. For example info 0, use 0 or use auxiliary/scanner/http/robots_txt                                                                                 
                                                                                                                                                                                            
msf6 > use 0                                                                                                                                                                                                     
msf6 auxiliary(scanner/http/robots_txt) > set rhosts 192.212.106.3                                                                                                                                               
rhosts => 192.212.106.3                                                                                                                                                                                          
msf6 auxiliary(scanner/http/robots_txt) > run

[*] [192.212.106.3] /robots.txt found
[+] Contents of Robots.txt:
User-agent: *
Disallow: /cgi-bin/
Disallow: Disallow: /junk/

User-agent: BadBot
Disallow: /no-badbot-dir/

[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf6 auxiliary(scanner/http/robots_txt) 
```

[[curl]]

```
root@INE:~# curl http://192.212.106.3/cgi-bin/
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>403 Forbidden</title>
</head><body>
<h1>Forbidden</h1>
<p>You don't have permission to access /cgi-bin/
on this server.<br />
</p>
<hr>
<address>Apache/2.4.18 (Ubuntu) Server at 192.212.106.3 Port 80</address>
</body></html>
```
