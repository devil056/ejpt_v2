#### ifconfig (8)         - configure a network interface

This is the other command we can use if incase we are unable to run the ip command. These both commands perform the same operation the only difference being the ip is modern approach while ifconfig is the traditional approach.

```
└─$ ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.0.2.10  netmask 255.255.255.0  broadcast 10.0.2.255
        inet6 fe80::a00:27ff:fe3f:e41  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:3f:0e:41  txqueuelen 1000  (Ethernet)
        RX packets 5011  bytes 703206 (686.7 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 5055  bytes 404508 (395.0 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 4  bytes 240 (240.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 4  bytes 240 (240.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

