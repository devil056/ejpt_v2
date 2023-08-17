What is metasploit framework?

- The metasploit framework (MSF) is an open source, robust penetration testing and exploitation framework that is used by penetration testers and security researchers wordwide.
- It provides penetration testers with a robust infrastructure required to automate every stage of the penetration testing lie cycle.
- It is also used to develop and test exploits and has one of the world's largest database of public,tested exploits
- The metasploit framework is designed to be modular, allowing for new functionality to be implemented with ease.

[Metasploit Github](https://github.com/rapid7/metasploit-framework)

Metasploit editions:
- Metasploit Pro (Commercial)
- Metasploit Express (Commercial)
- Metasploit Framework (Community)

Essential terminology

- Interface - methods of interacting with the metasploit framework
- Module - Pieces of code that perform a particular task, an example of a module is an exploit.
- Vulnerability -  Weakness or flaw in a computer system or network that can be exploited
- Exploit - Piece of code/module that is used to take advantage of a vulnerability within a system, service or application
- Payload - Piece of code delivered to the target system by an exploit with the objective of executing arbitrary commands or providing remote access to an attacker
- Listener - A utility that listens for an incoming connection from a target.

Metasploit Interfaces:
- The metasploit framework console (msfconsole) is an easy to use all in one interface that provides you with access to all the functionality of the metasploit framework.
- The metasploit framework command line interface (msfcli) is a command line utility that is used to facilitate the creation of autimation scripts that utilize metasploit modules
	- It can be used to redirect output from other tools in to msfcli and viceversa
	- Note : MSFcli was discontinued in 2015, however, the same functionality can be leveraged through msfconsole.
- Metasploit community edition is a web based GUI front end for the metasploit framework that simplifies network discovery and vulnerability identification.
- Armitage is a free Java based GUI front end for the Metasploit framework that simplifies network discovery, exploitation and post exploitation.

Metasploit framework architecture:
- A module in the context of msf us a piece of code that can be utilized by the msf
- The msf libraries facilitate the execution of modules without having to write the code necessary in order to execute them.

![[Pasted image 20230726053444.png]]

Msf Modules:
- Exploit - A module that is used to take advantage of vulnerability and is typically paired with a payload
- Payload - Code that is delivered by MSF and remotely executed on the target after successful exploitation. An example of a payload is a reverse shell that initiates a connection from the target system back to the attacker.
- Encoder -  Used to encode payloads in order to avoid AV detection. For example, shikata_ga_nai is used to encode Windows payloads
- NOPS - Used to ensure that payloads sizes are consistent and ensure the stability of a payload when executed
- Auxiliary - A module that is used to perform additional functionality like port scanning and enumeration.

Msf payload types:
- Non staged payload - payload that is sent to the target system as is along with the exploit
- Satged payload -  A staged payload is sent to the target in two parts, whereby,
	- The first part (stager) contains a payload that is used to establish a reverse connection back to the attacker, download the second part of the payload(stage) and execute it.
		- Stagers - Stagers are typically used to establish a stable communication channel between the attacker and the target, after which a stage payload is downloaded and executed on the target system.
		- Stage - Payload components that are downloaded by the stager.
- Meterpreter paylaod - the meterpreter payload is an advanced multi functional payload that is executed in memory on the target system making it difficult to detect. It communicates over a stager socket and provides an attacker with an intercative command interpreter on the target system that facilitates the execution of system commands, file system navigation, keylogging and much more.

MSF File system structure
The msf file system system is organized in a simple and easy to understand format and is organized into various directories.
- msf stores modules under the /usr/share/metasploit-framework/modules
- and the user specified modules are stored in director ~/.ms4/modules for linux

Penetration testing with the metasploit framework

- The msf can be used to perform and automate various tasks that fall under the penetration testing life cycle
- In order to understand how we can leverage the msf for penetration testing, we need to explore the various phases of a penetration test and their respective techniques and objectives.
- We can adopt the PTES (Penetration Testing Execution Standard) as a roadmap to understanding the various phases that make up a penetration test and how Metasploit can be integrated in to each phase.

penetration testing phase | Metasploit framework implementation
:--: | :--:
Information gathering and enumeration | Auxiliary modules
Vulnerability scanning | auziliary modules , Nessus
Exploitation | Exploit modules and payloads
Post exploitation | Meterpreter
Privilege escalation | Post exploitation modules , Meterpreter
Maintaining Persistent access | Post exploitation modules, persistence

Installing and configuring the metaspoit framework:

- The MSF is distributed by Rapid7 and can be downloaded and installed as a standalone package on both Windows and Linux.
- In this course we will be utilizing the Metasploit Framework on Linux and our preferred distribution of choice is Kali Linux
- MSF and its required dependencies come pre packaged with Kali linux which saves us from the tedious process of installing MSF manually.
- The Metasploit Framework Database (msfdb) is an integral part of the Metasploit framework and is used to keep track of all your assessments, host data and scans etc.
- The metasploit framework uses PostgreSQL as the primary database server, as a result, we will also need to ensure that the postgreSQL database service is running and configured correctly.
- The metasploit framework database also facilitates the importantion and storage of scan results from various third party tools like Nmap and Nessus.

Installation steps:
- Update our repositories and upgrade our Metasploit Framework to the latest version
- Start and enable the PostgreSQL database service
- Initialize the Metasploit Framework database (msfdb)
- Launch msfconsole.

install command :  `sudo apt-get update && sudo apt-get install metasploit-framework -y`
postgre start and msfconsole: `sudo systemctl enable postgresql`
to start postgres : `sudo systemctl start postgresql`
to check the status of postgres: `sudo systemctl status postgresql`
interacting with the metasploit database : `sudo msfdb`
start database :  `sudo msfdb init`
launch the metasploit: `msfconsole`
database status with metasploit : `db_status`

I got an error while initializing the database and had to refresh the collation version in order to fix the error I had to refreh the collation version of the database by the following steps:
1. sudo -i -u postgres
2. psql
3. ALTER DATABASE postgres REFRESH COLLATION VERSION;

By running the same exact commands we will be able to fix the issue.

reinitializing the postgres database just to be safe :
```
┌──(kali㉿kali)-[~]
└─$ sudo msfdb reinit
[i] Database already started
[+] Dropping databases 'msf'
[+] Dropping databases 'msf_test'
[+] Dropping database user 'msf'
[+] Deleting configuration file /usr/share/metasploit-framework/config/database.yml
[+] Stopping database
[+] Starting database
[+] Creating database user 'msf'
[+] Creating databases 'msf'
[+] Creating databases 'msf_test'
[+] Creating configuration file '/usr/share/metasploit-framework/config/database.yml'
[+] Creating initial database schema

```

```
┌──(kali㉿kali)-[~]
└─$ msfconsole             
       =[ metasploit v6.3.25-dev                          ]
+ -- --=[ 2332 exploits - 1219 auxiliary - 413 post       ]
+ -- --=[ 1385 payloads - 46 encoders - 11 nops           ]
+ -- --=[ 9 evasion                                       ]

Metasploit tip: You can pivot connections over sessions 
started with the ssh_login modules
Metasploit Documentation: https://docs.metasploit.com/

msf6 > db_status
[*] Connected to msf. Connection type: postgresql.
```

msfconsole fundamentals:

- Before we can start using the metasploit framework for penetration testng, we need to get an understanding of how to use the msfconsole
- The metasploit framework console (msfconsole) is an easy to use all in one interface that provides you with the access to all the functionality of the metasploit framework.
- We will be utilizing the msfconsole as our primary msf interface for the rest of the course.

Things we need to be have a clear understanding about:
1. How to search for modules
2. How to select modules
3. How to configure module options and variables
4. How to search for payloads
5. Managing sessions
6. Additional functionality
7. Saving our configuration.

Msf module variables:
- MSF modules will typically require information like the target and host IP address and port in order to initiate a remote exploit/connection.
- These options can be configured through the use of the MSF variables
- MSFconsole allows you to set both the local variables or global variable values.

Variable | Purpose
:--: | :--:
LHOST | This variable is used to store the IP address of the attacker's system
LPORT | This variable is used to store the port number on the attackers system that will be used to receive a reverse connection
RHOST | This variable is used to store the IP address of the target system/server
RHOSTS | This variable is used to specify the IP addresses of multiple target systems or network ranges
RPORT | This variable stores the port number that we are targeting on the target system.


You can start off launching the help menu by typing the command `help` in msfconsole.
help - to see all the available commands and options in msfconsole
version - to see the version of the msfconsole we are currently running 
show all - to list all the exploits, nops and other options categories as well
show exploits - to see all the available exploits in msf
If we want to see the available options for a command we can simply type '-h' at the end of the command let us say simply `show -h` we will be listed out with the options that we can use the show with.
```
msf6 > show -h
[*] Valid parameters for the "show" command are: all, encoders, nops, exploits, payloads, auxiliary, post, plugins, info, options, favorites
[*] Additional module-specific parameters are: missing, advanced, evasion, targets, actions
```

we can search for any module in msf using the command `search <keyword>`
```
msf6 > search portscan

Matching Modules
================

   #  Name                                              Disclosure Date  Rank    Check  Description
   -  ----                                              ---------------  ----    -----  -----------
   0  auxiliary/scanner/portscan/ftpbounce                               normal  No     FTP Bounce Port Scanner
   1  auxiliary/scanner/natpmp/natpmp_portscan                           normal  No     NAT-PMP External Port Scanner
   2  auxiliary/scanner/sap/sap_router_portscanner                       normal  No     SAPRouter Port Scanner
   3  auxiliary/scanner/portscan/xmas                                    normal  No     TCP "XMas" Port Scanner
   4  auxiliary/scanner/portscan/ack                                     normal  No     TCP ACK Firewall Scanner
   5  auxiliary/scanner/portscan/tcp                                     normal  No     TCP Port Scanner
   6  auxiliary/scanner/portscan/syn                                     normal  No     TCP SYN Port Scanner
   7  auxiliary/scanner/http/wordpress_pingback_access                   normal  No     Wordpress Pingback Locator


Interact with a module by name or index. For example info 7, use 7 or use auxiliary/scanner/http/wordpress_pingback_access
```

Incase if we are interested in running a particular module we have to load it before we can run it and the way that we can do it either by giving the item number from the search or by copy pasting the module path from the search list. Both the ways works just fine.

```
msf6> use auxiliary/scanner/portscan/tcp 

or

msf6> use 5
msf6 auxiliary(scanner/portscan/tcp) > 
```

And in order to configure the options we need to see first what are the available options for that we can use the command below

```
msf6 auxiliary(scanner/portscan/tcp) > show options

Module options (auxiliary/scanner/portscan/tcp):

   Name         Current Setting  Required  Description
   ----         ---------------  --------  -----------
   CONCURRENCY  10               yes       The number of concurrent ports to check per host
   DELAY        0                yes       The delay between connections, per thread, in milliseconds
   JITTER       0                yes       The delay jitter factor (maximum value by which to +/- DELAY) in milliseconds.
   PORTS        1-10000          yes       Ports to scan (e.g. 22-25,80,110-900)
   RHOSTS                        yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   THREADS      1                yes       The number of concurrent threads (max one per host)
   TIMEOUT      1000             yes       The socket connect timeout in milliseconds


View the full module info with the info, or info -d command.
```

or simply

```
msf6 auxiliary(scanner/portscan/tcp) > options

Module options (auxiliary/scanner/portscan/tcp):

   Name         Current Setting  Required  Description
   ----         ---------------  --------  -----------
   CONCURRENCY  10               yes       The number of concurrent ports to check per host
   DELAY        0                yes       The delay between connections, per thread, in milliseconds
   JITTER       0                yes       The delay jitter factor (maximum value by which to +/- DELAY) in milliseconds.
   PORTS        1-10000          yes       Ports to scan (e.g. 22-25,80,110-900)
   RHOSTS                        yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   THREADS      1                yes       The number of concurrent threads (max one per host)
   TIMEOUT      1000             yes       The socket connect timeout in milliseconds


View the full module info with the info, or info -d command.
```

as we can see we have multiple options and most of them are already on default if we are to set a option we need to do as below:

```
msf6 auxiliary(scanner/portscan/tcp) > set rhosts 10.0.2.5
rhosts => 10.0.2.5
msf6 auxiliary(scanner/portscan/tcp) > set ports 1-1000
ports => 1-1000
msf6 auxiliary(scanner/portscan/tcp) > show options

Module options (auxiliary/scanner/portscan/tcp):

   Name         Current Setting  Required  Description
   ----         ---------------  --------  -----------
   CONCURRENCY  10               yes       The number of concurrent ports to check per host
   DELAY        0                yes       The delay between connections, per thread, in milliseconds
   JITTER       0                yes       The delay jitter factor (maximum value by which to +/- DELAY) in milliseconds.
   PORTS        1-1000           yes       Ports to scan (e.g. 22-25,80,110-900)
   RHOSTS       10.0.2.5         yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   THREADS      1                yes       The number of concurrent threads (max one per host)
   TIMEOUT      1000             yes       The socket connect timeout in milliseconds


View the full module info with the info, or info -d command.
```

Once we are satisfied with the options we can run the module using the command run or exploit

```
msf6 auxiliary(scanner/portscan/tcp) > run
^C
[*] 10.0.2.5:             - Caught interrupt from the console...
[*] Auxiliary module execution completed
```

To interrupt the current running module we can use the keyboard shortcut ctrl+C and in order to unload the module use the command back

```
msf6 auxiliary(scanner/portscan/tcp) > back
msf6 >
```

Search command options:

```
Usage: search [<options>] [<keywords>:<value>]

Prepending a value with '-' will exclude any matching results.
If no options or keywords are provided, cached results are displayed.


OPTIONS:

    -h, --help                      Help banner
    -I, --ignore                    Ignore the command if the only match has the same name as the search
    -o, --output <filename>         Send output to a file in csv format
    -r, --sort-descending <column>  Reverse the order of search results to descending order
    -S, --filter <filter>           Regex pattern used to filter search results
    -s, --sort-ascending <column>   Sort search results by the specified column in ascending order
    -u, --use                       Use module if there is one result

Keywords:
  aka              :  Modules with a matching AKA (also-known-as) name
  author           :  Modules written by this author
  arch             :  Modules affecting this architecture
  bid              :  Modules with a matching Bugtraq ID
  cve              :  Modules with a matching CVE ID
  edb              :  Modules with a matching Exploit-DB ID
  check            :  Modules that support the 'check' method
  date             :  Modules with a matching disclosure date
  description      :  Modules with a matching description
  fullname         :  Modules with a matching full name
  mod_time         :  Modules with a matching modification date
  name             :  Modules with a matching descriptive name
  path             :  Modules with a matching path
  platform         :  Modules affecting this platform
  port             :  Modules with a matching port
  rank             :  Modules with a matching rank (Can be descriptive (ex: 'good') or numeric with comparison operators (ex: 'gte400'))
  ref              :  Modules with a matching ref
  reference        :  Modules with a matching reference
  target           :  Modules affecting this target
  type             :  Modules of a specific type (exploit, payload, auxiliary, encoder, evasion, post, or nop)

Supported search columns:
  rank             :  Sort modules by their exploitabilty rank
  date             :  Sort modules by their disclosure date. Alias for disclosure_date
  disclosure_date  :  Sort modules by their disclosure date
  name             :  Sort modules by their name
  type             :  Sort modules by their type
  check            :  Sort modules by whether or not they have a check method

Examples:
  search cve:2009 type:exploit
  search cve:2009 type:exploit platform:-linux
  search cve:2009 -s name
  search type:exploit -s type -r
```

using the search command options let us say we want to search the exploit modules for windows from the year 2020 the command would be as follows

```
msf6 > search cve:2020 type:exploit platform:windows

Matching Modules
================

   #   Name                                                                    Disclosure Date  Rank       Check  Description
   -   ----                                                                    ---------------  ----       -----  -----------
   0   exploit/windows/local/cve_2020_0787_bits_arbitrary_file_move            2020-03-10       excellent  Yes    Background Intelligent Transfer Service Arbitrary File Move Privilege Elevation Vulnerability
   1   exploit/windows/nimsoft/nimcontroller_bof                               2020-02-05       excellent  Yes    CA Unified Infrastructure Management Nimsoft 7.80 - Remote Buffer Overflow
   2   exploit/windows/local/cve_2020_17136                                    2020-03-10       normal     Yes    CVE-2020-1170 Cloud Filter Arbitrary File Creation EOP
   3   exploit/windows/http/cayin_xpost_sql_rce                                2020-06-04       excellent  Yes    Cayin xPost wayfinder_seqid SQLi to RCE
   4   exploit/windows/local/anyconnect_lpe                                    2020-08-05       excellent  Yes    Cisco AnyConnect Privilege Escalations (CVE-2020-3153 and CVE-2020-3433)
   5   exploit/windows/local/druva_insync_insynccphwnet64_rcp_type_5_priv_esc  2020-02-25       excellent  Yes    Druva inSync inSyncCPHwnet64.exe RPC Type 5 Privilege Escalation
   6   exploit/windows/http/exchange_ecp_viewstate                             2020-02-11       excellent  Yes    Exchange Control Panel ViewState Deserialization
   7   exploit/multi/browser/firefox_jit_use_after_free                        2020-11-18       manual     No     Firefox MCallGetProperty Write Side Effects Use After Free Exploit
   8   exploit/windows/http/flexdotnetcms_upload_exec                          2020-09-28       excellent  Yes    FlexDotnetCMS Arbitrary ASP File Upload
   9   exploit/windows/local/gog_galaxyclientservice_privesc                   2020-04-28       excellent  Yes    GOG GalaxyClientService Privilege Escalation
   10  exploit/windows/http/git_lfs_rce                                        2020-11-04       excellent  No     Git Remote Code Execution via git-lfs (CVE-2020-27955)
   11  exploit/multi/http/gitea_git_hooks_rce                                  2020-10-07       excellent  Yes    Gitea Git Hooks Remote Code Execution
   12  exploit/multi/http/gogs_git_hooks_rce                                   2020-10-07       excellent  Yes    Gogs Git Hooks Remote Code Execution
   13  exploit/multi/browser/chrome_jscreate_sideeffect                        2020-02-19       manual     No     Google Chrome 80 JSCreate side-effect type confusion exploit
   14  exploit/multi/browser/chrome_simplifiedlowering_overflow                2020-11-19       manual     No     Google Chrome versions before 87.0.4280.88 integer overflow during SimplfiedLowering phase
   15  exploit/windows/misc/hp_operations_agent_coda_8c                        2012-07-09       normal     Yes    HP Operations Agent Opcode coda.exe 0x8c Buffer Overflow
   16  exploit/windows/http/hpe_sim_76_amf_deserialization                     2020-12-15       excellent  Yes    HPE Systems Insight Manager AMF Deserialization RCE
   17  exploit/multi/http/horizontcms_upload_exec                              2020-09-24       excellent  Yes    HorizontCMS Arbitrary PHP File Upload
   18  exploit/multi/scada/inductive_ignition_rce                              2020-06-11       excellent  Yes    Inductive Automation Ignition Remote Code Execution
   19  exploit/windows/http/desktopcentral_deserialization                     2020-03-05       great      Yes    ManageEngine Desktop Central Java Deserialization
   20  exploit/multi/http/opmanager_sumpdu_deserialization                     2021-07-26       excellent  Yes    ManageEngine OpManager SumPDU Java Deserialization
```

The list is really long but the above description and sample output paints the picture more or less.

Now let us assume that we want search for the eternalblue module

```
msf6 > search eternalblue

Matching Modules
================

   #  Name                                      Disclosure Date  Rank     Check  Description
   -  ----                                      ---------------  ----     -----  -----------
   0  exploit/windows/smb/ms17_010_eternalblue  2017-03-14       average  Yes    MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption
   1  exploit/windows/smb/ms17_010_psexec       2017-03-14       normal   Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
   2  auxiliary/admin/smb/ms17_010_command      2017-03-14       normal   No     MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Command Execution
   3  auxiliary/scanner/smb/smb_ms17_010                         normal   No     MS17-010 SMB RCE Detection
   4  exploit/windows/smb/smb_doublepulsar_rce  2017-04-14       great    Yes    SMB DOUBLEPULSAR Remote Code Execution


Interact with a module by name or index. For example info 4, use 4 or use exploit/windows/smb/smb_doublepulsar_rce

msf6 > use 0
[*] No payload configured, defaulting to windows/x64/meterpreter/reverse_tcp
msf6 exploit(windows/smb/ms17_010_eternalblue) >
```

As we see there is an option stating that there is no payload that was set so it was defaulted to one `windows/x64/meterpreter/reverse_tcp` . Now let us see the options as we loaded the exploit.

```
msf6 exploit(windows/smb/ms17_010_eternalblue) > show options

Module options (exploit/windows/smb/ms17_010_eternalblue):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   RHOSTS                          yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT          445              yes       The target port (TCP)
   SMBDomain                       no        (Optional) The Windows domain to use for authentication. Only affects Windows Server 2008 R2, Windows 7, Windows Embedded Standard 7 target machines.
   SMBPass                         no        (Optional) The password for the specified username
   SMBUser                         no        (Optional) The username to authenticate as
   VERIFY_ARCH    true             yes       Check if remote architecture matches exploit Target. Only affects Windows Server 2008 R2, Windows 7, Windows Embedded Standard 7 target machines.
   VERIFY_TARGET  true             yes       Check if remote OS matches exploit Target. Only affects Windows Server 2008 R2, Windows 7, Windows Embedded Standard 7 target machines.


Payload options (windows/x64/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     10.0.2.4         yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic Target



View the full module info with the info, or info -d command.
```

Now we have more options that needs to be set for the module and the payload as well. If we are not satisfied with the payload and want to downgrade it to 32bit one as well we can do so using the set command.

```
msf6> set payload payload/windows/x64/meterpreter/reverse_tcp
```

we can list all the payloads for the exploit using the show payloads 

```
msf6 exploit(windows/smb/ms17_010_eternalblue) > show payloads

Compatible Payloads
===================

   #   Name                                                Disclosure Date  Rank    Check  Description
   -   ----                                                ---------------  ----    -----  -----------
   0   payload/generic/custom                                               normal  No     Custom Payload
   1   payload/generic/shell_bind_aws_ssm                                   normal  No     Command Shell, Bind SSM (via AWS API)
   2   payload/generic/shell_bind_tcp                                       normal  No     Generic Command Shell, Bind TCP Inline
   3   payload/generic/shell_reverse_tcp                                    normal  No     Generic Command Shell, Reverse TCP Inline
   4   payload/generic/ssh/interact                                         normal  No     Interact with Established SSH Connection
   5   payload/windows/x64/custom/bind_ipv6_tcp                             normal  No     Windows shellcode stage, Windows x64 IPv6 Bind TCP Stager
   6   payload/windows/x64/custom/bind_ipv6_tcp_uuid                        normal  No     Windows shellcode stage, Windows x64 IPv6 Bind TCP Stager with UUID Support
   7   payload/windows/x64/custom/bind_named_pipe                           normal  No     Windows shellcode stage, Windows x64 Bind Named Pipe Stager
   8   payload/windows/x64/custom/bind_tcp                                  normal  No     Windows shellcode stage, Windows x64 Bind TCP Stager
   9   payload/windows/x64/custom/bind_tcp_rc4                              normal  No     Windows shellcode stage, Bind TCP Stager (RC4 Stage Encryption, Metasm)
   10  payload/windows/x64/custom/bind_tcp_uuid                             normal  No     Windows shellcode stage, Bind TCP Stager with UUID Support (Windows x64)
   11  payload/windows/x64/custom/reverse_http                              normal  No     Windows shellcode stage, Windows x64 Reverse HTTP Stager (wininet)
   12  payload/windows/x64/custom/reverse_https                             normal  No     Windows shellcode stage, Windows x64 Reverse HTTP Stager (wininet)
   13  payload/windows/x64/custom/reverse_named_pipe                        normal  No     Windows shellcode stage, Windows x64 Reverse Named Pipe (SMB) Stager
   14  payload/windows/x64/custom/reverse_tcp                               normal  No     Windows shellcode stage, Windows x64 Reverse TCP Stager
   15  payload/windows/x64/custom/reverse_tcp_rc4                           normal  No     Windows shellcode stage, Reverse TCP Stager (RC4 Stage Encryption, Metasm)
   16  payload/windows/x64/custom/reverse_tcp_uuid                          normal  No     Windows shellcode stage, Reverse TCP Stager with UUID Support (Windows x64)
   17  payload/windows/x64/custom/reverse_winhttp                           normal  No     Windows shellcode stage, Windows x64 Reverse HTTP Stager (winhttp)
   18  payload/windows/x64/custom/reverse_winhttps                          normal  No     Windows shellcode stage, Windows x64 Reverse HTTPS Stager (winhttp)
   19  payload/windows/x64/exec                                             normal  No     Windows x64 Execute Command
   20  payload/windows/x64/loadlibrary                                      normal  No     Windows x64 LoadLibrary Path
   21  payload/windows/x64/messagebox                                       normal  No     Windows MessageBox x64
   22  payload/windows/x64/meterpreter/bind_ipv6_tcp                        normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 IPv6 Bind TCP Stager
   23  payload/windows/x64/meterpreter/bind_ipv6_tcp_uuid                   normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 IPv6 Bind TCP Stager with UUID Support
   24  payload/windows/x64/meterpreter/bind_named_pipe                      normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Bind Named Pipe Stager
   25  payload/windows/x64/meterpreter/bind_tcp                             normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Bind TCP Stager
   26  payload/windows/x64/meterpreter/bind_tcp_rc4                         normal  No     Windows Meterpreter (Reflective Injection x64), Bind TCP Stager (RC4 Stage Encryption, Metasm)
   27  payload/windows/x64/meterpreter/bind_tcp_uuid                        normal  No     Windows Meterpreter (Reflective Injection x64), Bind TCP Stager with UUID Support (Windows x64)
   28  payload/windows/x64/meterpreter/reverse_http                         normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse HTTP Stager (wininet)
   29  payload/windows/x64/meterpreter/reverse_https                        normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse HTTP Stager (wininet)
   30  payload/windows/x64/meterpreter/reverse_named_pipe                   normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse Named Pipe (SMB) Stager
   31  payload/windows/x64/meterpreter/reverse_tcp                          normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse TCP Stager
   32  payload/windows/x64/meterpreter/reverse_tcp_rc4                      normal  No     Windows Meterpreter (Reflective Injection x64), Reverse TCP Stager (RC4 Stage Encryption, Metasm)
   33  payload/windows/x64/meterpreter/reverse_tcp_uuid                     normal  No     Windows Meterpreter (Reflective Injection x64), Reverse TCP Stager with UUID Support (Windows x64)
   34  payload/windows/x64/meterpreter/reverse_winhttp                      normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse HTTP Stager (winhttp)
   35  payload/windows/x64/meterpreter/reverse_winhttps                     normal  No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse HTTPS Stager (winhttp)
   36  payload/windows/x64/peinject/bind_ipv6_tcp                           normal  No     Windows Inject Reflective PE Files, Windows x64 IPv6 Bind TCP Stager
   37  payload/windows/x64/peinject/bind_ipv6_tcp_uuid                      normal  No     Windows Inject Reflective PE Files, Windows x64 IPv6 Bind TCP Stager with UUID Support
   38  payload/windows/x64/peinject/bind_named_pipe                         normal  No     Windows Inject Reflective PE Files, Windows x64 Bind Named Pipe Stager
   39  payload/windows/x64/peinject/bind_tcp                                normal  No     Windows Inject Reflective PE Files, Windows x64 Bind TCP Stager
   40  payload/windows/x64/peinject/bind_tcp_rc4                            normal  No     Windows Inject Reflective PE Files, Bind TCP Stager (RC4 Stage Encryption, Metasm)
   41  payload/windows/x64/peinject/bind_tcp_uuid                           normal  No     Windows Inject Reflective PE Files, Bind TCP Stager with UUID Support (Windows x64)
   42  payload/windows/x64/peinject/reverse_named_pipe                      normal  No     Windows Inject Reflective PE Files, Windows x64 Reverse Named Pipe (SMB) Stager
   43  payload/windows/x64/peinject/reverse_tcp                             normal  No     Windows Inject Reflective PE Files, Windows x64 Reverse TCP Stager
   44  payload/windows/x64/peinject/reverse_tcp_rc4                         normal  No     Windows Inject Reflective PE Files, Reverse TCP Stager (RC4 Stage Encryption, Metasm)
   45  payload/windows/x64/peinject/reverse_tcp_uuid                        normal  No     Windows Inject Reflective PE Files, Reverse TCP Stager with UUID Support (Windows x64)
   46  payload/windows/x64/pingback_reverse_tcp                             normal  No     Windows x64 Pingback, Reverse TCP Inline
   47  payload/windows/x64/powershell_bind_tcp                              normal  No     Windows Interactive Powershell Session, Bind TCP
   48  payload/windows/x64/powershell_reverse_tcp                           normal  No     Windows Interactive Powershell Session, Reverse TCP
   49  payload/windows/x64/powershell_reverse_tcp_ssl                       normal  No     Windows Interactive Powershell Session, Reverse TCP SSL
   50  payload/windows/x64/shell/bind_ipv6_tcp                              normal  No     Windows x64 Command Shell, Windows x64 IPv6 Bind TCP Stager
   51  payload/windows/x64/shell/bind_ipv6_tcp_uuid                         normal  No     Windows x64 Command Shell, Windows x64 IPv6 Bind TCP Stager with UUID Support
   52  payload/windows/x64/shell/bind_named_pipe                            normal  No     Windows x64 Command Shell, Windows x64 Bind Named Pipe Stager
   53  payload/windows/x64/shell/bind_tcp                                   normal  No     Windows x64 Command Shell, Windows x64 Bind TCP Stager
   54  payload/windows/x64/shell/bind_tcp_rc4                               normal  No     Windows x64 Command Shell, Bind TCP Stager (RC4 Stage Encryption, Metasm)
   55  payload/windows/x64/shell/bind_tcp_uuid                              normal  No     Windows x64 Command Shell, Bind TCP Stager with UUID Support (Windows x64)
   56  payload/windows/x64/shell/reverse_tcp                                normal  No     Windows x64 Command Shell, Windows x64 Reverse TCP Stager
   57  payload/windows/x64/shell/reverse_tcp_rc4                            normal  No     Windows x64 Command Shell, Reverse TCP Stager (RC4 Stage Encryption, Metasm)
   58  payload/windows/x64/shell/reverse_tcp_uuid                           normal  No     Windows x64 Command Shell, Reverse TCP Stager with UUID Support (Windows x64)
   59  payload/windows/x64/shell_bind_tcp                                   normal  No     Windows x64 Command Shell, Bind TCP Inline
   60  payload/windows/x64/shell_reverse_tcp                                normal  No     Windows x64 Command Shell, Reverse TCP Inline
   61  payload/windows/x64/vncinject/bind_ipv6_tcp                          normal  No     Windows x64 VNC Server (Reflective Injection), Windows x64 IPv6 Bind TCP Stager
   62  payload/windows/x64/vncinject/bind_ipv6_tcp_uuid                     normal  No     Windows x64 VNC Server (Reflective Injection), Windows x64 IPv6 Bind TCP Stager with UUID Support
   63  payload/windows/x64/vncinject/bind_named_pipe                        normal  No     Windows x64 VNC Server (Reflective Injection), Windows x64 Bind Named Pipe Stager
   64  payload/windows/x64/vncinject/bind_tcp                               normal  No     Windows x64 VNC Server (Reflective Injection), Windows x64 Bind TCP Stager
   65  payload/windows/x64/vncinject/bind_tcp_rc4                           normal  No     Windows x64 VNC Server (Reflective Injection), Bind TCP Stager (RC4 Stage Encryption, Metasm)
   66  payload/windows/x64/vncinject/bind_tcp_uuid                          normal  No     Windows x64 VNC Server (Reflective Injection), Bind TCP Stager with UUID Support (Windows x64)
   67  payload/windows/x64/vncinject/reverse_http                           normal  No     Windows x64 VNC Server (Reflective Injection), Windows x64 Reverse HTTP Stager (wininet)
   68  payload/windows/x64/vncinject/reverse_https                          normal  No     Windows x64 VNC Server (Reflective Injection), Windows x64 Reverse HTTP Stager (wininet)
   69  payload/windows/x64/vncinject/reverse_tcp                            normal  No     Windows x64 VNC Server (Reflective Injection), Windows x64 Reverse TCP Stager
   70  payload/windows/x64/vncinject/reverse_tcp_rc4                        normal  No     Windows x64 VNC Server (Reflective Injection), Reverse TCP Stager (RC4 Stage Encryption, Metasm)
   71  payload/windows/x64/vncinject/reverse_tcp_uuid                       normal  No     Windows x64 VNC Server (Reflective Injection), Reverse TCP Stager with UUID Support (Windows x64)
   72  payload/windows/x64/vncinject/reverse_winhttp                        normal  No     Windows x64 VNC Server (Reflective Injection), Windows x64 Reverse HTTP Stager (winhttp)
   73  payload/windows/x64/vncinject/reverse_winhttps                       normal  No     Windows x64 VNC Server (Reflective Injection), Windows x64 Reverse HTTPS Stager (winhttp)
```

We can use the command `sessions` to see the active sessions in the msfconsole.

```
msf6 exploit(windows/smb/ms17_010_eternalblue) > sessions

Active sessions
===============

No active sessions.
```

connect command :

```
msf6 > connect -h
Usage: connect [options] <host> <port>

Communicate with a host, similar to interacting via netcat, taking advantage of
any configured session pivoting.

OPTIONS:

    -c, --comm <comm>               Specify which Comm to use.
    -C, --crlf                      Try to use CRLF for EOL sequence.
    -h, --help                      Help banner.
    -i, --send-contents <file>      Send the contents of a file.
    -p, --proxies <proxies>         List of proxies to use.
    -P, --source-port <port>        Specify source port.
    -S, --source-address <address>  Specify source address.
    -s, --ssl                       Connect with SSL.
    -u, --udp                       Switch to a UDP socket.
    -w, --timeout <seconds>         Specify connect timeout.
    -z, --try-connection            Just try to connect, then return.
```

```
msf6> connect 192.168.0.1 80
```

Inorder to set the global variable in msfconsole use `setg` instead of `set` command.

Creating and managing workspaces:

- Workspaces allow you to keep track of all your hosts, scans and activities and are extremely useful when conducting penetration tests as they allow you to sort and organize your data based on the target or organization.
- MSFConsole provides you with the ability to create, manage and switch between multiple workspaces depending on your requirements
- We will be using workspaces to organize our assessments as we progress through the course.

Workspace:

```
msf6 > db_status
[*] Connected to msf. Connection type: postgresql.
msf6 > workspace -h
Usage:
    workspace          List workspaces
    workspace [name]   Switch workspace

OPTIONS:

    -a, --add <name>          Add a workspace.
    -d, --delete <name>       Delete a workspace.
    -D, --delete-all          Delete all workspaces.
    -h, --help                Help banner.
    -l, --list                List workspaces.
    -r, --rename <old> <new>  Rename a workspace.
    -S, --search <name>       Search for a workspace.
    -v, --list-verbose        List workspaces verbosely.
```

We will be performing penetration testing for multiple organizations in order to make sure we are organized and all the variables related to the organization are not mixed up we use workspaces. And as we have see all the results are stored to a postgresql database so before we go ahead and start off with the workspaces we need to check the database connection first.

```
msf6 > workspace
* default
msf6 > hosts

Hosts
=====

address  mac  name  os_name  os_flavor  os_sp  purpose  info  comments
-------  ---  ----  -------  ---------  -----  -------  ----  --------

msf6 > workspace -a Test
[*] Added workspace: Test
[*] Workspace: Test
msf6 > workspace
  default
* Test
msf6 >
```