### Nessus

- Nessus is a proprietary vulnerability scanner developed by Tenable
- We can utilize Nessus to perform a vulnerability scan on a target system, after which, we can import the Nessus results in to MSF for analysis and exploitation
- Nessus automates the process of identifying vulnerabilities and also provides us with information pertinent to a vulnerability like the CVE code.
- We can use the free version of Nessus (Nessus Essentials), which allows us to scan upto 16 IPs.

Try to get the activation code first from the website not from the installed application. Use the nessus essentials as it is the free version and we only get to scan 16 IP address with the free version incase if we want to scan more we need to obtain a new license. Once the installation is success we can start the nessus using the following commands. Also we need to setup a local login id and password. For my application the same creds are used for both kali machine and nessus. 

Command to start nessus

```
/bin/systemctl start nessusd.service
```

once it is lunched we can try to access the nessus portal from browser using [Nessus default gateway](https://kali:8834/) 

to stop nessus

```
/bin/systemctl stop nessusd.service
```

We can perform the basic scan and all ports if you wish and then export the result as Nessus. From the metasploit framework we can import the downloaded results and continue with the exploitation.

`db_import <path/to/nessus/file>` using this we can import the results of the scan.