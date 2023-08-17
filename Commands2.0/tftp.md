#### tftp

IPv4 Trivial File Transfer Protocol client

```
IPv4 Trivial File Transfer Protocol client

Usage: tftp [-4][-6][-v][-l][-m mode] [host [port]] [-c command]
tftp-hpa 5.2
Commands may be abbreviated.  Commands are:

connect         connect to remote tftp
mode            set file transfer mode
put             send file
get             receive file
quit            exit tftp
verbose         toggle verbose mode
trace           toggle packet tracing
literal         toggle literal mode, ignore ':' in file name
status          show current status
binary          set mode to octet
ascii           set mode to netascii
rexmt           set per-packet transmission timeout
timeout         set total retransmission timeout
?               print help information
help            print help information
tftp> 

```

#### Basic tftp connect command:
```
tftp <target-ip> <port-no.>
```
