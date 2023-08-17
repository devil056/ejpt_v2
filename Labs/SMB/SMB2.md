### SMB2

`ris:PriceTag3` [[Servers and Services#SMB|SMB]] 
`ris:Tools` [[Hack2.0/NMAP|NMAP]] [[rpcclient]] [[metasploit]] [[enum4linux]] [[smbclient]]

Questions

1. Find the OS version of samba server using rpcclient.
2. Find the OS version of samba server using enum4Linux.
3. Find the server description of samba server using smbclient.
4. Is NT LM 0.12 (SMBv1) dialects supported by the samba server? Use appropriate nmap script.
5. Is SMB2 protocol supported by the samba server? Use smb2 metasploit module.
6. List all users that exists on the samba server  using appropriate nmap script.
7. List all users that exists on the samba server  using smb_enumusers metasploit modules.
8. List all users that exists on the samba server  using enum4Linux.
9. List all users that exists on the samba server  using rpcclient.
10. Find SID of user “admin” using rpcclient.

[[ip]]

```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
3900: eth0@if3901: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:06 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.6/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
3903: eth1@if3904: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:20:e7:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.32.231.2/24 brd 192.32.231.255 scope global eth1
       valid_lft forever preferred_lft forever
```

[[ping]]

```
root@attackdefense:~# ping -c 5 192.32.231.3
PING 192.32.231.3 (192.32.231.3) 56(84) bytes of data.
64 bytes from 192.32.231.3: icmp_seq=1 ttl=64 time=0.101 ms
64 bytes from 192.32.231.3: icmp_seq=2 ttl=64 time=0.045 ms
64 bytes from 192.32.231.3: icmp_seq=3 ttl=64 time=0.058 ms
64 bytes from 192.32.231.3: icmp_seq=4 ttl=64 time=0.034 ms
64 bytes from 192.32.231.3: icmp_seq=5 ttl=64 time=0.034 ms

--- 192.32.231.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 101ms
rtt min/avg/max/mdev = 0.034/0.054/0.101/0.025 ms
```

[[Hack2.0/NMAP#General Scan|NMAP general scan]]

```
root@attackdefense:~# nmap 192.32.231.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-08 15:56 UTC
Nmap scan report for target-1 (192.32.231.3)
Host is up (0.0000090s latency).
Not shown: 998 closed ports
PORT    STATE SERVICE
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
MAC Address: 02:42:C0:20:E7:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.19 seconds
```

[[Hack2.0/NMAP#Service version|NMAP service version info]]

```
root@attackdefense:~# nmap -p 139,445 -sV 192.32.231.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-08 15:56 UTC
Nmap scan report for target-1 (192.32.231.3)
Host is up (0.000032s latency).

PORT    STATE SERVICE     VERSION
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: RECONLABS)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: RECONLABS)
MAC Address: 02:42:C0:20:E7:03 (Unknown)
Service Info: Host: SAMBA-RECON

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.48 seconds
```

[[rpcclient#login without password|rpcclient null session]]

```
root@attackdefense:~# rpcclient -U "" -N 192.32.231.3
rpcclient $> srvinfo
        SAMBA-RECON    Wk Sv PrQ Unx NT SNT samba.recon.lab
        platform_id     :       500
        os version      :       6.1
        server type     :       0x809a03
```

[[enum4linux#Getting os info|OS versioning using enum4linux]]

```
root@attackdefense:~# enum4linux -o 192.32.231.3
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Sat Jul  8 16:02:18 2023

 ========================== 
|    Target Information    |
 ========================== 
Target ........... 192.32.231.3
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ==================================================== 
|    Enumerating Workgroup/Domain on 192.32.231.3    |
 ==================================================== 
[+] Got domain/workgroup name: RECONLABS

 ===================================== 
|    Session Check on 192.32.231.3    |
 ===================================== 
[+] Server 192.32.231.3 allows sessions using username '', password ''

 =========================================== 
|    Getting domain SID for 192.32.231.3    |
 =========================================== 
Domain Name: RECONLABS
Domain Sid: (NULL SID)
[+] Can't determine if host is part of domain or part of a workgroup

 ====================================== 
|    OS information on 192.32.231.3    |
 ====================================== 
Use of uninitialized value $os_info in concatenation (.) or string at ./enum4linux.pl line 464.
[+] Got OS info for 192.32.231.3 from smbclient: 
[+] Got OS info for 192.32.231.3 from srvinfo:
        SAMBA-RECON    Wk Sv PrQ Unx NT SNT samba.recon.lab
        platform_id     :       500
        os version      :       6.1
        server type     :       0x809a03
enum4linux complete on Sat Jul  8 16:02:18 2023
```

[[smbclient#no password access|null session using smbclient]]

```
root@attackdefense:~# smbclient -L 192.32.231.3 -N 

        Sharename       Type      Comment
        ---------       ----      -------
        public          Disk      
        john            Disk      
        aisha           Disk      
        emma            Disk      
        everyone        Disk      
        IPC$            IPC       IPC Service (samba.recon.lab)
Reconnecting with SMB1 for workgroup listing.

        Server               Comment
        ---------            -------

        Workgroup            Master
        ---------            -------
        RECONLABS            SAMBA-RECON
```

[[Hack2.0/NMAP#Specific script|NMAP dialect finding]]

```
root@attackdefense:~# nmap -p 445 --script smb-protocols 192.32.231.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-08 16:07 UTC
Nmap scan report for target-1 (192.32.231.3)
Host is up (0.000037s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds
MAC Address: 02:42:C0:20:E7:03 (Unknown)

Host script results:
| smb-protocols: 
|   dialects: 
|     NT LM 0.12 (SMBv1) [dangerous, but default]
|     2.02
|     2.10
|     3.00
|     3.02
|_    3.11

Nmap done: 1 IP address (1 host up) scanned in 0.36 seconds
```

[[metasploit#Basic commands|Version check using the msfconsole]]

```
msf5 > search smb

Matching Modules
================

   #    Name                                                            Disclosure Date  Rank       Check  Description
   -    ----                                                            ---------------  ----       -----  -----------
   1    auxiliary/admin/mssql/mssql_enum_domain_accounts                                 normal     No     Microsoft SQL Server SUSER_SNAME Windows Domain Account Enumeration
   2    auxiliary/admin/mssql/mssql_enum_domain_accounts_sqli                            normal     No     Microsoft SQL Server SQLi SUSER_SNAME Windows Domain Account Enumeration
   3    auxiliary/admin/mssql/mssql_ntlm_stealer                                         normal     Yes    Microsoft SQL Server NTLM Stealer
   4    auxiliary/admin/mssql/mssql_ntlm_stealer_sqli                                    normal     No     Microsoft SQL Server SQLi NTLM Stealer
   5    auxiliary/admin/oracle/ora_ntlm_stealer                         2009-04-07       normal     No     Oracle SMB Relay Code Execution
   6    auxiliary/admin/smb/check_dir_file                                               normal     Yes    SMB Scanner Check File/Directory Utility
   7    auxiliary/admin/smb/delete_file                                                  normal     Yes    SMB File Delete Utility
   8    auxiliary/admin/smb/download_file                                                normal     Yes    SMB File Download Utility
   9    auxiliary/admin/smb/list_directory                                               normal     No     SMB Directory Listing Utility
   10   auxiliary/admin/smb/ms17_010_command                            2017-03-14       normal     Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Command Execution
   11   auxiliary/admin/smb/psexec_command                                               normal     Yes    Microsoft Windows Authenticated Administration Utility
   12   auxiliary/admin/smb/psexec_ntdsgrab                                              normal     No     PsExec NTDS.dit And SYSTEM Hive Download Utility
   13   auxiliary/admin/smb/samba_symlink_traversal                                      normal     No     Samba Symlink Directory Traversal
   14   auxiliary/admin/smb/upload_file                                                  normal     Yes    SMB File Upload Utility
   15   auxiliary/admin/smb/webexec_command                                              normal     Yes    WebEx Remote Command Execution Utility
   16   auxiliary/docx/word_unc_injector                                                 normal     No     Microsoft Word UNC Path Injector
   17   auxiliary/dos/samba/read_nttrans_ea_list                                         normal     No     Samba read_nttrans_ea_list Integer Overflow
   18   auxiliary/dos/sap/sap_soap_rfc_eps_delete_file                                   normal     Yes    SAP SOAP EPS_DELETE_FILE File Deletion
   19   auxiliary/dos/smb/smb_loris                                     2017-06-29       normal     No     SMBLoris NBSS Denial of Service
   20   auxiliary/dos/windows/smb/ms05_047_pnp                                           normal     No     Microsoft Plug and Play Service Registry Overflow
   21   auxiliary/dos/windows/smb/ms06_035_mailslot                     2006-07-11       normal     No     Microsoft SRV.SYS Mailslot Write Corruption
   22   auxiliary/dos/windows/smb/ms06_063_trans                                         normal     No     Microsoft SRV.SYS Pipe Transaction No Null
   23   auxiliary/dos/windows/smb/ms09_001_write                                         normal     No     Microsoft SRV.SYS WriteAndX Invalid DataOffset
   24   auxiliary/dos/windows/smb/ms09_050_smb2_negotiate_pidhigh                        normal     No     Microsoft SRV2.SYS SMB Negotiate ProcessID Function Table Dereference
   25   auxiliary/dos/windows/smb/ms09_050_smb2_session_logoff                           normal     No     Microsoft SRV2.SYS SMB2 Logoff Remote Kernel NULL Pointer Dereference
   26   auxiliary/dos/windows/smb/ms10_006_negotiate_response_loop                       normal     No     Microsoft Windows 7 / Server 2008 R2 SMB Client Infinite Loop
   27   auxiliary/dos/windows/smb/ms10_054_queryfs_pool_overflow                         normal     No     Microsoft Windows SRV.SYS SrvSmbQueryFsInformation Pool Overflow DoS
   28   auxiliary/dos/windows/smb/ms11_019_electbowser                                   normal     No     Microsoft Windows Browser Pool DoS
   29   auxiliary/dos/windows/smb/rras_vls_null_deref                   2006-06-14       normal     No     Microsoft RRAS InterfaceAdjustVLSPointers NULL Dereference
   30   auxiliary/dos/windows/smb/vista_negotiate_stop                                   normal     No     Microsoft Vista SP0 SMB Negotiate Protocol DoS
   31   auxiliary/fileformat/multidrop                                                   normal     No     Windows SMB Multi Dropper
   32   auxiliary/fileformat/odt_badodt                                 2018-05-01       normal     No     LibreOffice 6.03 /Apache OpenOffice 4.1.5 Malicious ODT File Generator
   33   auxiliary/fuzzers/smb/smb2_negotiate_corrupt                                     normal     No     SMB Negotiate SMB2 Dialect Corruption
   34   auxiliary/fuzzers/smb/smb_create_pipe                                            normal     No     SMB Create Pipe Request Fuzzer
   35   auxiliary/fuzzers/smb/smb_create_pipe_corrupt                                    normal     No     SMB Create Pipe Request Corruption
   36   auxiliary/fuzzers/smb/smb_negotiate_corrupt                                      normal     No     SMB Negotiate Dialect Corruption
   37   auxiliary/fuzzers/smb/smb_ntlm1_login_corrupt                                    normal     No     SMB NTLMv1 Login Request Corruption
   38   auxiliary/fuzzers/smb/smb_tree_connect                                           normal     No     SMB Tree Connect Request Fuzzer
   39   auxiliary/fuzzers/smb/smb_tree_connect_corrupt                                   normal     No     SMB Tree Connect Request Corruption
   40   auxiliary/gather/konica_minolta_pwd_extract                                      normal     Yes    Konica Minolta Password Extractor
   41   auxiliary/scanner/sap/sap_smb_relay                                              normal     Yes    SAP SMB Relay Abuse
   42   auxiliary/scanner/sap/sap_soap_rfc_eps_get_directory_listing                     normal     Yes    SAP SOAP RFC EPS_GET_DIRECTORY_LISTING Directories Information Disclosure
   43   auxiliary/scanner/sap/sap_soap_rfc_pfl_check_os_file_existence                   normal     Yes    SAP SOAP RFC PFL_CHECK_OS_FILE_EXISTENCE File Existence Check
   44   auxiliary/scanner/sap/sap_soap_rfc_rzl_read_dir                                  normal     Yes    SAP SOAP RFC RZL_READ_DIR_LOCAL Directory Contents Listing
   45   auxiliary/scanner/smb/impacket/dcomexec                         2018-03-19       normal     Yes    DCOM Exec
   46   auxiliary/scanner/smb/impacket/secretsdump                                       normal     Yes    DCOM Exec
   47   auxiliary/scanner/smb/impacket/wmiexec                          2018-03-19       normal     Yes    WMI Exec
   48   auxiliary/scanner/smb/pipe_auditor                                               normal     Yes    SMB Session Pipe Auditor
   49   auxiliary/scanner/smb/pipe_dcerpc_auditor                                        normal     Yes    SMB Session Pipe DCERPC Auditor
   50   auxiliary/scanner/smb/psexec_loggedin_users                                      normal     Yes    Microsoft Windows Authenticated Logged In Users Enumeration
   51   auxiliary/scanner/smb/smb1                                                       normal     Yes    SMBv1 Protocol Detection
   52   auxiliary/scanner/smb/smb2                                                       normal     Yes    SMB 2.0 Protocol Detection
   53   auxiliary/scanner/smb/smb_enum_gpp                                               normal     Yes    SMB Group Policy Preference Saved Passwords Enumeration
   54   auxiliary/scanner/smb/smb_enumshares                                             normal     Yes    SMB Share Enumeration
   55   auxiliary/scanner/smb/smb_enumusers                                              normal     Yes    SMB User Enumeration (SAM EnumUsers)
   56   auxiliary/scanner/smb/smb_enumusers_domain                                       normal     Yes    SMB Domain User Enumeration
   57   auxiliary/scanner/smb/smb_login                                                  normal     Yes    SMB Login Check Scanner
   58   auxiliary/scanner/smb/smb_lookupsid                                              normal     Yes    SMB SID User Enumeration (LookupSid)
   59   auxiliary/scanner/smb/smb_ms17_010                                               normal     Yes    MS17-010 SMB RCE Detection
   60   auxiliary/scanner/smb/smb_uninit_cred                                            normal     Yes    Samba _netr_ServerPasswordSet Uninitialized Credential State
   61   auxiliary/scanner/smb/smb_version                                                normal     Yes    SMB Version Detection
   62   auxiliary/scanner/snmp/snmp_enumshares                                           normal     Yes    SNMP Windows SMB Share Enumeration
   63   auxiliary/server/capture/smb                                                     normal     No     Authentication Capture: SMB
   64   auxiliary/server/http_ntlmrelay                                                  normal     No     HTTP Client MS Credential Relayer
   65   auxiliary/spoof/nbns/nbns_response                                               normal     No     NetBIOS Name Service Spoofer
   66   exploit/linux/samba/chain_reply                                 2010-06-16       good       No     Samba chain_reply Memory Corruption (Linux x86)
   67   exploit/multi/http/struts_code_exec_classloader                 2014-03-06       manual     No     Apache Struts ClassLoader Manipulation Remote Code Execution
   68   exploit/multi/ids/snort_dce_rpc                                 2007-02-19       good       No     Snort 2 DCE/RPC Preprocessor Buffer Overflow
   69   exploit/netware/smb/lsass_cifs                                  2007-01-21       average    No     Novell NetWare LSASS CIFS.NLM Driver Stack Buffer Overflow
   70   exploit/osx/browser/safari_file_policy                          2011-10-12       normal     No     Apple Safari file:// Arbitrary Code Execution
   71   exploit/windows/browser/java_ws_arginject_altjvm                2010-04-09       excellent  No     Sun Java Web Start Plugin Command Line Argument Injection
   72   exploit/windows/browser/java_ws_double_quote                    2012-10-16       excellent  No     Sun Java Web Start Double Quote Injection
   73   exploit/windows/browser/java_ws_vmargs                          2012-02-14       excellent  No     Sun Java Web Start Plugin Command Line Argument Injection
   74   exploit/windows/browser/ms10_022_ie_vbscript_winhlp32           2010-02-26       great      No     MS10-022 Microsoft Internet Explorer Winhlp32.exe MsgBox Code Execution
   75   exploit/windows/fileformat/ms13_071_theme                       2013-09-10       excellent  No     MS13-071 Microsoft Windows Theme File Handling Arbitrary Code Execution
   76   exploit/windows/fileformat/ms14_060_sandworm                    2014-10-14       excellent  No     MS14-060 Microsoft Windows OLE Package Manager Code Execution
   77   exploit/windows/fileformat/ursoft_w32dasm                       2005-01-24       good       No     URSoft W32Dasm Disassembler Function Buffer Overflow
   78   exploit/windows/fileformat/vlc_smb_uri                          2009-06-24       great      No     VideoLAN Client (VLC) Win32 smb:// URI Buffer Overflow
   79   exploit/windows/http/generic_http_dll_injection                 2015-03-04       manual     No     Generic Web Application DLL Injection
   80   exploit/windows/misc/hp_dataprotector_cmd_exec                  2014-11-02       excellent  Yes    HP Data Protector 8.10 Remote Command Execution
   81   exploit/windows/misc/hp_dataprotector_install_service           2011-11-02       excellent  Yes    HP Data Protector 6.10/6.11/6.20 Install Service
   82   exploit/windows/oracle/extjob                                   2007-01-01       excellent  Yes    Oracle Job Scheduler Named Pipe Command Execution
   83   exploit/windows/scada/ge_proficy_cimplicity_gefebt              2014-01-23       excellent  Yes    GE Proficy CIMPLICITY gefebt.exe Remote Code Execution
   84   exploit/windows/smb/generic_smb_dll_injection                   2015-03-04       manual     No     Generic DLL Injection From Shared Resource
   85   exploit/windows/smb/group_policy_startup                        2015-01-26       manual     No     Group Policy Script Execution From Shared Resource
   86   exploit/windows/smb/ipass_pipe_exec                             2015-01-21       excellent  Yes    IPass Control Pipe Remote Command Execution
   87   exploit/windows/smb/ms03_049_netapi                             2003-11-11       good       No     MS03-049 Microsoft Workstation Service NetAddAlternateComputerName Overflow
   88   exploit/windows/smb/ms04_007_killbill                           2004-02-10       low        No     MS04-007 Microsoft ASN.1 Library Bitstring Heap Overflow
   89   exploit/windows/smb/ms04_011_lsass                              2004-04-13       good       No     MS04-011 Microsoft LSASS Service DsRolerUpgradeDownlevelServer Overflow
   90   exploit/windows/smb/ms04_031_netdde                             2004-10-12       good       No     MS04-031 Microsoft NetDDE Service Overflow
   91   exploit/windows/smb/ms05_039_pnp                                2005-08-09       good       Yes    MS05-039 Microsoft Plug and Play Service Overflow
   92   exploit/windows/smb/ms06_025_rasmans_reg                        2006-06-13       good       No     MS06-025 Microsoft RRAS Service RASMAN Registry Overflow
   93   exploit/windows/smb/ms06_025_rras                               2006-06-13       average    No     MS06-025 Microsoft RRAS Service Overflow
   94   exploit/windows/smb/ms06_040_netapi                             2006-08-08       good       No     MS06-040 Microsoft Server Service NetpwPathCanonicalize Overflow
   95   exploit/windows/smb/ms06_066_nwapi                              2006-11-14       good       No     MS06-066 Microsoft Services nwapi32.dll Module Exploit
   96   exploit/windows/smb/ms06_066_nwwks                              2006-11-14       good       No     MS06-066 Microsoft Services nwwks.dll Module Exploit
   97   exploit/windows/smb/ms06_070_wkssvc                             2006-11-14       manual     No     MS06-070 Microsoft Workstation Service NetpManageIPCConnect Overflow
   98   exploit/windows/smb/ms07_029_msdns_zonename                     2007-04-12       manual     No     MS07-029 Microsoft DNS RPC Service extractQuotedChar() Overflow (SMB)
   99   exploit/windows/smb/ms08_067_netapi                             2008-10-28       great      Yes    MS08-067 Microsoft Server Service Relative Path Stack Corruption
   100  exploit/windows/smb/ms09_050_smb2_negotiate_func_index          2009-09-07       good       No     MS09-050 Microsoft SRV2.SYS SMB Negotiate ProcessID Function Table Dereference
   101  exploit/windows/smb/ms10_046_shortcut_icon_dllloader            2010-07-16       excellent  No     Microsoft Windows Shell LNK Code Execution
   102  exploit/windows/smb/ms10_061_spoolss                            2010-09-14       excellent  No     MS10-061 Microsoft Print Spooler Service Impersonation Vulnerability
   103  exploit/windows/smb/ms15_020_shortcut_icon_dllloader            2015-03-10       excellent  No     Microsoft Windows Shell LNK Code Execution
   104  exploit/windows/smb/ms17_010_eternalblue                        2017-03-14       average    No     MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption
   105  exploit/windows/smb/ms17_010_eternalblue_win8                   2017-03-14       average    No     MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption for Win8+
   106  exploit/windows/smb/ms17_010_psexec                             2017-03-14       normal     No     MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
   107  exploit/windows/smb/netidentity_xtierrpcpipe                    2009-04-06       great      No     Novell NetIdentity Agent XTIERRPCPIPE Named Pipe Buffer Overflow
   108  exploit/windows/smb/psexec                                      1999-01-01       manual     No     Microsoft Windows Authenticated User Code Execution
   109  exploit/windows/smb/psexec_psh                                  1999-01-01       manual     No     Microsoft Windows Authenticated Powershell Command Execution
   110  exploit/windows/smb/smb_delivery                                2016-07-26       excellent  No     SMB Delivery
   111  exploit/windows/smb/smb_relay                                   2001-03-31       excellent  No     MS08-068 Microsoft Windows SMB Relay Code Execution
   112  exploit/windows/smb/timbuktu_plughntcommand_bof                 2009-06-25       great      No     Timbuktu PlughNTCommand Named Pipe Buffer Overflow
   113  exploit/windows/smb/webexec                                     2018-10-24       manual     No     WebExec Authenticated User Code Execution
   114  payload/windows/meterpreter/reverse_named_pipe                                   normal     No     Windows Meterpreter (Reflective Injection), Windows x86 Reverse Named Pipe (SMB) Stager
   115  payload/windows/x64/meterpreter/reverse_named_pipe                               normal     No     Windows Meterpreter (Reflective Injection x64), Windows x64 Reverse Named Pipe (SMB) Stager
   116  post/linux/busybox/smb_share_root                                                normal     No     BusyBox SMB Sharing
   117  post/linux/gather/mount_cifs_creds                                               normal     No     Linux Gather Saved mount.cifs/mount.smbfs Credentials
   118  post/windows/escalate/droplnk                                                    normal     No     Windows Escalate SMB Icon LNK Dropper
   119  post/windows/gather/credentials/gpp                                              normal     No     Windows Gather Group Policy Preference Saved Passwords
   120  post/windows/gather/enum_shares                                                  normal     No     Windows Gather SMB Share Enumeration via Registry
   121  post/windows/gather/netlm_downgrade                                              normal     No     Windows NetLM Downgrade Attack
   122  post/windows/gather/word_unc_injector                                            normal     No     Windows Gather Microsoft Office Word UNC Path Injector


msf5 > use  auxiliary/scanner/smb/smb2
msf5 auxiliary(scanner/smb/smb2) > options

Module options (auxiliary/scanner/smb/smb2):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   RHOSTS                    yes       The target address range or CIDR identifier
   RPORT    445              yes       The target port (TCP)
   THREADS  1                yes       The number of concurrent threads

msf5 auxiliary(scanner/smb/smb2) > set rhosts 192.32.231.3
rhosts => 192.32.231.3
msf5 auxiliary(scanner/smb/smb2) > exploit

[+] 192.32.231.3:445      - 192.32.231.3 supports SMB 2 [dialect 255.2] and has been online for 3703696 hours
[*] 192.32.231.3:445      - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/smb/smb2) > 
```

[[Hack2.0/NMAP#Specific script|NMAP enumerating users using the NMAP]]

```
root@attackdefense:~# nmap -p445 --script smb-enum-users 192.32.231.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-08 16:13 UTC
Nmap scan report for target-1 (192.32.231.3)
Host is up (0.000058s latency).

PORT    STATE SERVICE
445/tcp open  microsoft-ds
MAC Address: 02:42:C0:20:E7:03 (Unknown)

Host script results:
| smb-enum-users: 
|   SAMBA-RECON\admin (RID: 1005)
|     Full name:   
|     Description: 
|     Flags:       Normal user account
|   SAMBA-RECON\aisha (RID: 1004)
|     Full name:   
|     Description: 
|     Flags:       Normal user account
|   SAMBA-RECON\elie (RID: 1002)
|     Full name:   
|     Description: 
|     Flags:       Normal user account
|   SAMBA-RECON\emma (RID: 1003)
|     Full name:   
|     Description: 
|     Flags:       Normal user account
|   SAMBA-RECON\john (RID: 1000)
|     Full name:   
|     Description: 
|     Flags:       Normal user account
|   SAMBA-RECON\shawn (RID: 1001)
|     Full name:   
|     Description: 
|_    Flags:       Normal user account

Nmap done: 1 IP address (1 host up) scanned in 0.41 seconds
```

[[enum4linux#Getting users|users list from enum4linux]]

```
root@attackdefense:~# enum4linux -U 192.32.231.3
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Sat Jul  8 16:16:05 2023

 ========================== 
|    Target Information    |
 ========================== 
Target ........... 192.32.231.3
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ==================================================== 
|    Enumerating Workgroup/Domain on 192.32.231.3    |
 ==================================================== 
[+] Got domain/workgroup name: RECONLABS

 ===================================== 
|    Session Check on 192.32.231.3    |
 ===================================== 
[+] Server 192.32.231.3 allows sessions using username '', password ''

 =========================================== 
|    Getting domain SID for 192.32.231.3    |
 =========================================== 
Domain Name: RECONLABS
Domain Sid: (NULL SID)
[+] Can't determine if host is part of domain or part of a workgroup

 ============================= 
|    Users on 192.32.231.3    |
 ============================= 
index: 0x1 RID: 0x3e8 acb: 0x00000010 Account: john     Name:   Desc: 
index: 0x2 RID: 0x3ea acb: 0x00000010 Account: elie     Name:   Desc: 
index: 0x3 RID: 0x3ec acb: 0x00000010 Account: aisha    Name:   Desc: 
index: 0x4 RID: 0x3e9 acb: 0x00000010 Account: shawn    Name:   Desc: 
index: 0x5 RID: 0x3eb acb: 0x00000010 Account: emma     Name:   Desc: 
index: 0x6 RID: 0x3ed acb: 0x00000010 Account: admin    Name:   Desc: 

user:[john] rid:[0x3e8]
user:[elie] rid:[0x3ea]
user:[aisha] rid:[0x3ec]
user:[shawn] rid:[0x3e9]
user:[emma] rid:[0x3eb]
user:[admin] rid:[0x3ed]
enum4linux complete on Sat Jul  8 16:16:05 2023
```

[[rpcclient#login without password|null session user enum using rpcclient]]

```
root@attackdefense:~# rpcclient -U "" -N 192.32.231.3
rpcclient $> enumdomusers
user:[john] rid:[0x3e8]
user:[elie] rid:[0x3ea]
user:[aisha] rid:[0x3ec]
user:[shawn] rid:[0x3e9]
user:[emma] rid:[0x3eb]
user:[admin] rid:[0x3ed]
rpcclient $> 
rpcclient $> lookupnames admin
admin S-1-5-21-4056189605-2085045094-1961111545-1005 (User: 1)
```

[[metasploit]]

```
msf5 > search smb_enumusers

Matching Modules
================

   #  Name                                        Disclosure Date  Rank    Check  Description
   -  ----                                        ---------------  ----    -----  -----------
   1  auxiliary/scanner/smb/smb_enumusers                          normal  Yes    SMB User Enumeration (SAM EnumUsers)
   2  auxiliary/scanner/smb/smb_enumusers_domain                   normal  Yes    SMB Domain User Enumeration


msf5 > use auxiliary/scanner/smb/smb_enumusers
msf5 auxiliary(scanner/smb/smb_enumusers) > options

Module options (auxiliary/scanner/smb/smb_enumusers):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   RHOSTS                      yes       The target address range or CIDR identifier
   SMBDomain  .                no        The Windows domain to use for authentication
   SMBPass                     no        The password for the specified username
   SMBUser                     no        The username to authenticate as
   THREADS    1                yes       The number of concurrent threads

msf5 auxiliary(scanner/smb/smb_enumusers) > set rhosts 192.32.231.3
rhosts => 192.32.231.3
msf5 auxiliary(scanner/smb/smb_enumusers) > run

[+] 192.32.231.3:139      - SAMBA-RECON [ john, elie, aisha, shawn, emma, admin ] ( LockoutTries=0 PasswordMin=5 )
[*] 192.32.231.3:         - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf5 auxiliary(scanner/smb/smb_enumusers) >
```
