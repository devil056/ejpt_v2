### nmblookup

NetBIOS over TCP/IP client used to lookup NetBIOS names

```
Usage: [-?fMRSTrAV] [-?|--help] [--usage] [-B|--broadcast=BROADCAST-ADDRESS]
        [-f|--flags] [-U|--unicast=STRING] [-M|--master-browser]
        [-R|--recursion] [-S|--status] [-T|--translate] [-r|--root-port]
        [-A|--lookup-by-ip] [-d|--debuglevel=DEBUGLEVEL]
        [-s|--configfile=CONFIGFILE] [-l|--log-basename=LOGFILEBASE]
        [-V|--version] [--option=name=value]
        [-O|--socket-options=SOCKETOPTIONS] [-n|--netbiosname=NETBIOSNAME]
        [-W|--workgroup=WORKGROUP] [-i|--scope=SCOPE] <NODE> ...
```

#### Basic Command:

```
nmblookup -A <target-ip>
```

