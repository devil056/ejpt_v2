### Stages of Hacking

`ris:PriceTag3` #intro #recon #passive-recon
`ris:Tools`  #host #robots #sitemap #wappalyzer/builtwith #whatweb #httrack #whois #netcraft #dnsrecon #dnsdumpster #wafw00f #sublist3r #amass #theHarvester #havipwned
`ris:Links`  N/A

There are 5 stages in hacking
1. Reconnaissance
2. Scanning and Enumeration
3. Gaining Access/ Exploitation
4. Maintaining Access/ Persistence
5. Clearing Tracks

### Reconnaissance: 

This is the first phase of pentest where we try to gather as much information as possible about the target system and the target organisation. There are two different types of information gathering techniques.
The more information that we have the more helpful it will be in the later stages of the pentest.

>**Passive Information Gathering**
```
In this phase we do not engage with the target or target network. We use the different open source information gathering tools to collect as much information as possible. The things that we are looking for in this phase are
1. IP Addresses
2. Directories hidden from the search engines
3. Names
4. Email Addresses
5. Phone numbers
6. Physical addresses
7. Web technologies being used
```

>**Active Information Gathering**
```
In this phase we actively enagage with the target system and try to do initial port scanning and try to get the versions of the services that are currently running on the system. And the number of devices that are active on the network and so on.
1. Discovering the open ports
2. Learning about the internal infrastructure of the target network/organisation
3. Enumerating the information from the target systems.
```


#### Passive recon:
Whenever we enter a url in the search bar and hit enter in the backend the url is converted into a IP address by the DNS server to which our operating system is configured to look at and we are redirected to the webpage. The url came into picture so that we do not have to remeber the IP address of the websites we visit. In this phase of information gathering let us assume that our target is 'hackersploit.org'. 

###### [[host|HOST Command]]

```
└─$ host hackersploit.org
hackersploit.org has address 172.67.202.99
hackersploit.org has address 104.21.44.180
hackersploit.org has IPv6 address 2606:4700:3031::6815:2cb4
hackersploit.org has IPv6 address 2606:4700:3036::ac43:ca63
hackersploit.org mail is handled by 0 _dc-mx.2c2a3526b376.hackersploit.org.
```

In this we can see that we have two IPv4 addresses and IPv6 address and the reason might be it is protected by a firewall and also we get the mail server.

###### robots.txt

When we are looking for information related to any webapplication the best start would be looking at robots.txt file. It is a text file that contains some entries in. When we use any search engine like Google,Bing or Yahoo the search engines crawl website and index the application. In case if it happens to be that there is no robots.txt file chances are that the search engines might index the potentially confindential directories or paths. So it would be in best interest to specify the directories and the paths that you do not want the search engines to index.

```
User-agent: *
Disallow: /wp-content/uploads/wpo-plugins-tables-list.json

# START YOAST BLOCK
# ---------------------------
User-agent: *
Disallow:

Sitemap: https://hackersploit.org/sitemap_index.xml
# ---------------------------
# END YOAST BLOCK
```

###### sitemap.xml / sitemaps.xml

It is similar to that of the robots.txt the purpose is to assist the search engines with an organised way of indexing the websites. We can check both the sitemap.xml and robots.txt simply by appending the 
/(robots.txt or sitemap.xml) to the url of the website.

###### builtwith and wappalyzer un built addons of browsers

using these two in built addons of the browser we can identify the different technologies used to build the websites.

###### [[Hack2.0/Commands2.0/whatweb|whatweb]]

```
└─$ whatweb hackersploit.org
http://hackersploit.org [301 Moved Permanently] Country[UNITED STATES][US], HTTPServer[cloudflare], IP[104.21.44.180], RedirectLocation[https://hackersploit.org/], UncommonHeaders[report-to,nel,cf-ray,alt-svc]
https://hackersploit.org/ [403 Forbidden] Country[UNITED STATES][US], HTML5, HTTPServer[cloudflare], IP[104.21.44.180], Title[403 Forbidden][Title element contains newline(s)!], UncommonHeaders[referrer-policy,x-turbo-charged-by,cf-cache-status,report-to,nel,cf-ray,alt-svc]
```

###### httrack

Using this tool we can download the files of a website on to our system and analyze it. We need to manually install this application in kali.

```
sudo apt install webhttrack
```

Once the application is install the downloading of the application is fairly straight forward, we just need to search for the application and launch it choose the language preference > projectName > Category and the path to the folder where you want the downloaded application files to be stored.

###### [[whois|whois enumeration]]

Whenever we are trying to pentest a website it would be in our best interest to know when it was registered and who registered it. So in order to get that information we can look in who.is or whois command in our terminal. 

```
└─$ whois hackersploit.org
Domain Name: hackersploit.org
Registry Domain ID: 77f8fe62a425487cbefef4bf7e27d2ec-LROR
Registrar WHOIS Server: whois.namecheap.com
Registrar URL: http://www.namecheap.com
Updated Date: 2022-12-22T11:20:08Z
Creation Date: 2018-04-05T11:27:07Z
Registry Expiry Date: 2024-04-05T11:27:07Z
Registrar: NameCheap, Inc.
Registrar IANA ID: 1068
Registrar Abuse Contact Email: abuse@namecheap.com
Registrar Abuse Contact Phone: +1.6613102107
Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
Registry Registrant ID: REDACTED FOR PRIVACY
Registrant Name: REDACTED FOR PRIVACY
Registrant Organization: Privacy service provided by Withheld for Privacy ehf
Registrant Street: REDACTED FOR PRIVACY
Registrant City: REDACTED FOR PRIVACY
Registrant State/Province: Capital Region
Registrant Postal Code: REDACTED FOR PRIVACY
Registrant Country: IS
Registrant Phone: REDACTED FOR PRIVACY
Registrant Phone Ext: REDACTED FOR PRIVACY
Registrant Fax: REDACTED FOR PRIVACY
Registrant Fax Ext: REDACTED FOR PRIVACY
Registrant Email: Please query the RDDS service of the Registrar of Record identified in this output for information on how to contact the Registrant, Admin, or Tech contact of the queried domain name.
Registry Admin ID: REDACTED FOR PRIVACY
Admin Name: REDACTED FOR PRIVACY
Admin Organization: REDACTED FOR PRIVACY
Admin Street: REDACTED FOR PRIVACY
Admin City: REDACTED FOR PRIVACY
Admin State/Province: REDACTED FOR PRIVACY
Admin Postal Code: REDACTED FOR PRIVACY
Admin Country: REDACTED FOR PRIVACY
Admin Phone: REDACTED FOR PRIVACY
Admin Phone Ext: REDACTED FOR PRIVACY
Admin Fax: REDACTED FOR PRIVACY
Admin Fax Ext: REDACTED FOR PRIVACY
Admin Email: Please query the RDDS service of the Registrar of Record identified in this output for information on how to contact the Registrant, Admin, or Tech contact of the queried domain name.
Registry Tech ID: REDACTED FOR PRIVACY
Tech Name: REDACTED FOR PRIVACY
Tech Organization: REDACTED FOR PRIVACY
Tech Street: REDACTED FOR PRIVACY
Tech City: REDACTED FOR PRIVACY
Tech State/Province: REDACTED FOR PRIVACY
Tech Postal Code: REDACTED FOR PRIVACY
Tech Country: REDACTED FOR PRIVACY
Tech Phone: REDACTED FOR PRIVACY
Tech Phone Ext: REDACTED FOR PRIVACY
Tech Fax: REDACTED FOR PRIVACY
Tech Fax Ext: REDACTED FOR PRIVACY
Tech Email: Please query the RDDS service of the Registrar of Record identified in this output for information on how to contact the Registrant, Admin, or Tech contact of the queried domain name.
Name Server: dee.ns.cloudflare.com
Name Server: jim.ns.cloudflare.com
DNSSEC: unsigned
URL of the ICANN Whois Inaccuracy Complaint Form: https://www.icann.org/wicf/
>>> Last update of WHOIS database: 2023-06-30T02:25:52Z <<<

For more information on Whois status codes, please visit https://icann.org/epp

Terms of Use: Access to Public Interest Registry WHOIS information is provided to assist persons in determining the contents of a domain name registration record in the Public Interest Registry registry database. The data in this record is provided by Public Interest Registry for informational purposes only, and Public Interest Registry does not guarantee its accuracy. This service is intended only for query-based access. You agree that you will use this data only for lawful purposes and that, under no circumstances will you use this data to (a) allow, enable, or otherwise support the transmission by e-mail, telephone, or facsimile of mass unsolicited, commercial advertising or solicitations to entities other than the data recipient's own existing customers; or (b) enable high volume, automated, electronic processes that send queries or data to the systems of Registry Operator, a Registrar, or Identity Digital except as reasonably necessary to register domain names or modify existing registrations. All rights reserved. Public Interest Registry reserves the right to modify these terms at any time. By submitting this query, you agree to abide by this policy.  The Registrar of Record identified in this output may have an RDDS service that can be queried for additional information on how to contact the Registrant, Admin, or Tech contact of the queried domain name.
```

`ris:Fire` please refer to the zonetransfer.me site as well.

###### Netcraft

We can get the ssl and tls certificate information on the website and the technologies used with the help of netcraft. We need to select the option what is the site running. It might also contain the information we previously were able to access by running the whois and wappalyzer but in a detailed and organised way e.g. [Netcraft-for-hackersploit](https://sitereport.netcraft.com/?url=https://hackersploit.org#ssl_table)

###### [[dnsrecon]] (passive)

The above are some of the different types of dnsrecords apart from these we have a large number of dnsrecords list we can refer to them simply with a google search. e.g.[dnsrecords](https://dnsdumpster.com/)

```
└─$ dnsrecon -d hackersploit.org
[*] std: Performing General Enumeration against: hackersploit.org...
[*] DNSSEC is configured for hackersploit.org
[*] DNSKEYs:
[*]     NSEC ZSK ECDSAP256SHA256 a09311112cf9138818cd2feae970ebbd 4d6a30f6088c25b325a39abbc5cd1197 aa098283e5aaf421177c2aa5d714992a 9957d1bcc18f98cd71f1f1806b65e148
[*]     NSEC KSk ECDSAP256SHA256 99db2cc14cabdc33d6d77da63a2f15f7 1112584f234e8d1dc428e39e8a4a97e1 aa271a555dc90701e17e2a4c4b6f120b 7c32d44f4ac02bd894cf2d4be7778a19
[*]      SOA dee.ns.cloudflare.com 172.64.32.93
[*]      SOA dee.ns.cloudflare.com 173.245.58.93
[*]      SOA dee.ns.cloudflare.com 108.162.192.93
[*]      SOA dee.ns.cloudflare.com 2803:f800:50::6ca2:c05d
[*]      SOA dee.ns.cloudflare.com 2606:4700:50::adf5:3a5d
[*]      SOA dee.ns.cloudflare.com 2a06:98c1:50::ac40:205d
[*]      NS jim.ns.cloudflare.com 173.245.59.125
[*]      Bind Version for 173.245.59.125 "2023.6.1"
[*]      NS jim.ns.cloudflare.com 108.162.193.125
[*]      Bind Version for 108.162.193.125 "2023.6.1"
[*]      NS jim.ns.cloudflare.com 172.64.33.125
[*]      Bind Version for 172.64.33.125 "2023.6.1"
[*]      NS jim.ns.cloudflare.com 2a06:98c1:50::ac40:217d
[*]      NS jim.ns.cloudflare.com 2606:4700:58::adf5:3b7d
[*]      NS jim.ns.cloudflare.com 2803:f800:50::6ca2:c17d
[*]      NS dee.ns.cloudflare.com 108.162.192.93
[*]      Bind Version for 108.162.192.93 "2023.6.1"
[*]      NS dee.ns.cloudflare.com 173.245.58.93
[*]      Bind Version for 173.245.58.93 "2023.6.1"
[*]      NS dee.ns.cloudflare.com 172.64.32.93
[*]      Bind Version for 172.64.32.93 "2023.6.1"
[*]      NS dee.ns.cloudflare.com 2a06:98c1:50::ac40:205d
[*]      NS dee.ns.cloudflare.com 2606:4700:50::adf5:3a5d
[*]      NS dee.ns.cloudflare.com 2803:f800:50::6ca2:c05d
[*]      MX _dc-mx.2c2a3526b376.hackersploit.org 198.54.120.212
[*]      A hackersploit.org 172.67.202.99
[*]      A hackersploit.org 104.21.44.180
[*]      AAAA hackersploit.org 2606:4700:3031::6815:2cb4
[*]      AAAA hackersploit.org 2606:4700:3036::ac43:ca63
[*]      TXT hackersploit.org v=spf1 a:my.hackersploit.org ~all
[*]      TXT hackersploit.org google-site-verification=TW0pQsFZ0xx3w4b7kysBV0UrcMq7fJFB-5Rz9h6GwkU
[*] Enumerating SRV Records
[+] 0 Records Found
```

###### dnsdumpster

We can get the same information that we collected by running the dnsrecon using dnsdumpster and it is a webbased tool. You can refer to [hackersploit-dnsdump](https://dnsdumpster.com/) and search for the url to get the required information.

###### [[wafw00f|firewall detection using wafw00f]]

This tool is used to identify the firewall used by the website and is inbuilt with kali.

```
└─$ wafw00f hackersploit.org

                ______
               /      \
              (  W00f! )
               \  ____/
               ,,    __            404 Hack Not Found
           |`-.__   / /                      __     __
           /"  _/  /_/                       \ \   / /
          *===*    /                          \ \_/ /  405 Not Allowed
         /     )__//                           \   /
    /|  /     /---`                        403 Forbidden
    \\/`   \ |                                 / _ \
    `\    /_\\_              502 Bad Gateway  / / \ \  500 Internal Error
      `_____``-`                             /_/   \_\

                        ~ WAFW00F : v2.2.0 ~
        The Web Application Firewall Fingerprinting Toolkit
    
[*] Checking https://hackersploit.org
[+] The site https://hackersploit.org is behind Cloudflare (Cloudflare Inc.) WAF.
[~] Number of requests: 2
```

```
└─$ wafw00f hackersploit.org -a

                ______
               /      \
              (  W00f! )
               \  ____/
               ,,    __            404 Hack Not Found
           |`-.__   / /                      __     __
           /"  _/  /_/                       \ \   / /
          *===*    /                          \ \_/ /  405 Not Allowed
         /     )__//                           \   /
    /|  /     /---`                        403 Forbidden
    \\/`   \ |                                 / _ \
    `\    /_\\_              502 Bad Gateway  / / \ \  500 Internal Error
      `_____``-`                             /_/   \_\

                        ~ WAFW00F : v2.2.0 ~
        The Web Application Firewall Fingerprinting Toolkit
    
[*] Checking https://hackersploit.org
[+] The site https://hackersploit.org is behind Cloudflare (Cloudflare Inc.) and/or LiteSpeed (LiteSpeed Technologies) WAF.
[+] Generic Detection results:
[*] The site https://hackersploit.org seems to be behind a WAF or some sort of security solution
[~] Reason: The server returns a different response code when an attack string is used.
Normal response code is "200", while the response code to cross-site scripting attack is "403"
[~] Number of requests: 5
```

###### [[sublist3r]]

It is a tool designed to enumerate subdomains of websites using OSINT. We can install it by running the command below. Also we can use the tool amass the main purpose is not to use the tool but to gather the information on subdomain enumeration. Incase if we fail to run the gather information using sublist3r try using the amass which comes pre packaged with the kali linux.

`ris:ErrorWarning` sublister is not working as expected
```
sudo apt install sublist3r
sudo apt-get install sublist3r
```

```
sublist3r -d <target-url>
```

`ris:Checkbox` amass is working fine.
```
└─$ amass enum --passive -d hackersploit.org
studio.community.hackersploit.org
cloud.hackersploit.org
_dc-mx.2c2a3526b376.hackersploit.org
videos.hackersploit.org
www.hackersploit.org
prm.hackersploit.org
hackersploit.org
forum.hackersploit.org
dc-8362e7e54e2a.hackersploit.org
demo.hackersploit.org
my.hackersploit.org
preview.community.hackersploit.org
community.hackersploit.org
new.hackersploit.org
apps.community.hackersploit.org
test.hackersploit.org

The enumeration has finished
```

###### Google dorks

```
site:*ine.com
doctype:pdf
filetype:
intitle:admin
inurl:
```

Google hacking is an easy way to get the required information. By mix and matching the above parameters we have n number of possibilies to get to the information we are looking for. Have a little patience while you are manually doing the google hacking. Also we can use the waybackmachine website tracker to check out the older version of websites. [Access link](https://securitytrails.com/blog/google-hacking-techniques)

###### [[theHarvester]]

```
└─$ theHarvester -d hackersploit.org -b brave,bing,duckduckgo,crtsh,rapiddns,sitedossier,subdomainfinderc99,threatminer,urlscan
[*] ASNS found: 1
--------------------
AS13335

[*] Interesting Urls found: 2
--------------------
https://hackersploit.org/
https://hackersploit.org/tryhackme-walkthroughs/

[*] IPs found: 11
-------------------
104.21.44.180
172.67.202.99
2606:4700:30::6812:3260
2606:4700:3031::6815:2cb4
2606:4700:3036::6812:3260
2606:4700:3036::6812:3360
2606:4700:3036::ac43:ca63
2a06:98c1:3120::3
2a06:98c1:3120::c
2a06:98c1:3121::3
2a06:98c1:3121::c
```

**[Check Password](https://haveibeenpwned.com/)** In case if your network is previously compromised and your password is cracked you can check if that is available publicly in this website. In order to make sure your password is strong use minimum 14 and above chars or really long sentences.