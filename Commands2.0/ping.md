#### ping

It is a type-8 echo request

```
─$ ping -h

Usage
  ping [options] <destination>

Options:
  <destination>      dns name or ip address
  -a                 use audible ping
  -A                 use adaptive ping
  -B                 sticky source address
  -c <count>         stop after <count> replies
  -C                 call connect() syscall on socket creation
  -D                 print timestamps
  -d                 use SO_DEBUG socket option
  -e <identifier>    define identifier for ping session, default is random for
                     SOCK_RAW and kernel defined for SOCK_DGRAM
                     Imply using SOCK_RAW (for IPv4 only for identifier 0)
  -f                 flood ping
  -h                 print help and exit
  -I <interface>     either interface name or address
  -i <interval>      seconds between sending each packet
  -L                 suppress loopback of multicast packets
  -l <preload>       send <preload> number of packages while waiting replies
  -m <mark>          tag the packets going out
  -M <pmtud opt>     define mtu discovery, can be one of <do|dont|want>
  -n                 no dns name resolution
  -O                 report outstanding replies
  -p <pattern>       contents of padding byte
  -q                 quiet output
  -Q <tclass>        use quality of service <tclass> bits
  -s <size>          use <size> as number of data bytes to be sent
  -S <size>          use <size> as SO_SNDBUF socket option value
  -t <ttl>           define time to live
  -U                 print user-to-user latency
  -v                 verbose output
  -V                 print version and exit
  -w <deadline>      reply wait <deadline> in seconds
  -W <timeout>       time to wait for response

IPv4 options:
  -4                 use IPv4
  -b                 allow pinging broadcast
  -R                 record route
  -T <timestamp>     define timestamp, can be one of <tsonly|tsandaddr|tsprespec>

IPv6 options:
  -6                 use IPv6
  -F <flowlabel>     define flow label, default is random
  -N <nodeinfo opt>  use icmp6 node info query, try <help> as argument

For more details see ping(8)
```

The both the versions of the command are used to check if the targe system is active by sending the ICMP packets to the targets and by analysing the responses we get from the ports. Linux based systems respond back to the ICMP pings but windows based systems does not respond back to the ICMP ping.

```
ping <target-ip>
```

```
ping -c 5 <target-ip>
```

Successful connect

```
└─$ ping 10.0.2.3
PING 10.0.2.3 (10.0.2.3) 56(84) bytes of data.
64 bytes from 10.0.2.3: icmp_seq=1 ttl=255 time=0.090 ms
64 bytes from 10.0.2.3: icmp_seq=2 ttl=255 time=0.111 ms
64 bytes from 10.0.2.3: icmp_seq=3 ttl=255 time=0.108 ms
64 bytes from 10.0.2.3: icmp_seq=4 ttl=255 time=0.098 ms
64 bytes from 10.0.2.3: icmp_seq=5 ttl=255 time=0.108 ms
^C
--- 10.0.2.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4099ms
rtt min/avg/max/mdev = 0.090/0.103/0.111/0.007 ms

```

Unsuccessful connect

```
─$ ping 10.0.2.7
PING 10.0.2.7 (10.0.2.7) 56(84) bytes of data.
From 10.0.2.5 icmp_seq=1 Destination Host Unreachable
From 10.0.2.5 icmp_seq=2 Destination Host Unreachable
From 10.0.2.5 icmp_seq=3 Destination Host Unreachable
^C
--- 10.0.2.7 ping statistics ---
5 packets transmitted, 0 received, +3 errors, 100% packet loss, time 4071ms
pipe 4
```