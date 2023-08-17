`ris:Tools` [[Hack2.0/NMAP|NMAP]]

[[ip]]
```
root@attackdefense:~# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ip_vti0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN group default qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
1238: eth0@if1239: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:0a:01:00:09 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.1.0.9/16 brd 10.1.255.255 scope global eth0
       valid_lft forever preferred_lft forever
1241: eth1@if1242: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:c0:8b:81:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.139.129.2/24 brd 192.139.129.255 scope global eth1
       valid_lft forever preferred_lft forever
```

[[ping]]

```
root@attackdefense:~# ping -c 5 192.139.129.3
PING 192.139.129.3 (192.139.129.3) 56(84) bytes of data.
64 bytes from 192.139.129.3: icmp_seq=1 ttl=64 time=0.081 ms
64 bytes from 192.139.129.3: icmp_seq=2 ttl=64 time=0.055 ms
64 bytes from 192.139.129.3: icmp_seq=3 ttl=64 time=0.047 ms
64 bytes from 192.139.129.3: icmp_seq=4 ttl=64 time=0.030 ms
64 bytes from 192.139.129.3: icmp_seq=5 ttl=64 time=0.038 ms

--- 192.139.129.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 82ms
rtt min/avg/max/mdev = 0.030/0.050/0.081/0.018 ms
```

[[Hack2.0/NMAP#General Scan|NMAP]]
```
root@attackdefense:~# nmap 192.139.129.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-06 09:56 UTC
Nmap scan report for target-1 (192.139.129.3)
Host is up (0.0000090s latency).
All 1000 scanned ports on target-1 (192.139.129.3) are closed
MAC Address: 02:42:C0:8B:81:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.19 seconds
```

[[Hack2.0/NMAP#scan all ports|NMAP All port scan]]

```
root@attackdefense:~# nmap -p- 192.139.129.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-06 09:56 UTC
Nmap scan report for target-1 (192.139.129.3)
Host is up (0.0000090s latency).
Not shown: 65532 closed ports
PORT      STATE SERVICE
6421/tcp  open  nim-wan
41288/tcp open  unknown
55413/tcp open  unknown
MAC Address: 02:42:C0:8B:81:03 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 1.09 seconds
```

[[Hack2.0/NMAP#Service version|NMAP Service versions]] and [[Hack2.0/NMAP#OS detection|NMAP OS detection]]

```
root@attackdefense:~# nmap -p 6421,41288,55413 -sV -O 192.139.129.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-06 09:57 UTC
Nmap scan report for target-1 (192.139.129.3)
Host is up (0.000026s latency).

PORT      STATE SERVICE   VERSION
6421/tcp  open  mongodb   MongoDB 2.6.10
41288/tcp open  memcached Memcached
55413/tcp open  ftp       vsftpd 3.0.3
MAC Address: 02:42:C0:8B:81:03 (Unknown)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 2.6.32 (96%), Linux 3.2 - 4.9 (96%), Linux 2.6.32 - 3.10 (96%), Linux 3.4 - 3.10 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), Synology DiskStation Manager 5.2-5644 (94%), Netgear RAIDiator 4.2.28 (94%), Linux 2.6.32 - 2.6.35 (94%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop
Service Info: OS: Unix

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.11 seconds
```

Just for fun let us try running default scripts and see what we will get.

[[Hack2.0/NMAP#Script scan|NMAP Script running]]

```
root@attackdefense:~# nmap -p 6421,41288,55413 -sV -sC -O 192.139.129.3
Starting Nmap 7.70 ( https://nmap.org ) at 2023-07-06 09:57 UTC
Nmap scan report for target-1 (192.139.129.3)
Host is up (0.000030s latency).

PORT      STATE SERVICE   VERSION
6421/tcp  open  mongodb   MongoDB 2.6.10 2.6.10
| mongodb-databases: 
|   totalSize = 83886080.0
|   ok = 1.0
|   databases
|     1
|       name = admin
|       empty = true
|       sizeOnDisk = 1.0
|     0
|       name = local
|       empty = false
|_      sizeOnDisk = 83886080.0
| mongodb-info: 
|   MongoDB Build info
|     loaderFlags = -fPIC -pthread -Wl,-z,now -rdynamic
|     versionArray
|       1 = 6
|       2 = 10
|       3 = 0
|       0 = 2
|     allocator = tcmalloc
|     OpenSSLVersion = OpenSSL 1.0.2g  1 Mar 2016
|     debug = false
|     ok = 1.0
|     maxBsonObjectSize = 16777216
|     version = 2.6.10
|     bits = 64
|     gitVersion = nogitversion
|     sysInfo = Linux lgw01-12 3.19.0-25-generic #26~14.04.1-Ubuntu SMP Fri Jul 24 21:16:20 UTC 2015 x86_64 BOOST_LIB_VERSION=1_58
|     javascriptEngine = V8
|     compilerFlags = -Wnon-virtual-dtor -Woverloaded-virtual -fPIC -fno-strict-aliasing -ggdb -pthread -Wall -Wsign-compare -Wno-unused-function -Wno-unused-variable -Wno-maybe-uninitialized -Wno-unknown-pragmas -Winvalid-pch -pipe -Werror -O3 -Wno-unused-local-typedefs -Wno-unused-function -Wno-deprecated-declarations -fno-builtin-memcmp
|   Server status
|     process = mongod
|     cursors
|       totalNoTimeout = 0
|       totalOpen = 0
|       note = deprecated, use server status metrics
|       timedOut = 0
|       pinned = 0
|       clientCursors_size = 0
|     ok = 1.0
|     dur
|       earlyCommits = 0
|       commitsInWriteLock = 0
|       journaledMB = 0.0
|       timeMs
|         writeToJournal = 0
|         prepLogBuffer = 0
|         writeToDataFiles = 0
|         dt = 3066
|         remapPrivateView = 0
|       writeToDataFilesMB = 0.0
|       commits = 30
|       compression = 0.0
|     recordStats
|       accessesNotInMemory = 0
|       admin
|         accessesNotInMemory = 0
|         pageFaultExceptionsThrown = 0
|       pageFaultExceptionsThrown = 0
|       local
|         accessesNotInMemory = 0
|         pageFaultExceptionsThrown = 0
|     mem
|       resident = 44
|       virtual = 382
|       mappedWithJournal = 160
|       mapped = 80
|       supported = true
|       bits = 64
|     globalLock
|       activeClients
|         readers = 0
|         total = 0
|         writers = 0
|       totalTime = 565071000
|       lockTime = 20351
|       currentQueue
|         readers = 0
|         total = 0
|         writers = 0
|     extra_info
|       page_faults = 2
|       heap_usage_bytes = 62664048
|       note = fields vary by platform
|     uptime = 565.0
|     metrics
|       cursor
|         open
|           noTimeout = 0
|           total = 0
|           pinned = 0
|         timedOut = 0
|       queryExecutor
|         scannedObjects = 0
|         scanned = 0
|       record
|         moves = 0
|       storage
|         freelist
|           search
|             bucketExhausted = 0
|             requests = 6
|             scanned = 11
|       ttl
|         deletedDocuments = 0
|         passes = 9
|       operation
|         scanAndOrder = 0
|         fastmod = 0
|         idhack = 0
|       document
|         inserted = 1
|         returned = 0
|         updated = 0
|         deleted = 0
|       getLastError
|         wtimeouts = 0
|         wtime
|           num = 0
|           totalMillis = 0
|       repl
|         apply
|           batches
|             num = 0
|             totalMillis = 0
|           ops = 0
|         buffer
|           maxSizeBytes = 268435456
|           count = 0
|           sizeBytes = 0
|         network
|           getmores
|             num = 0
|             totalMillis = 0
|           readersCreated = 0
|           ops = 0
|           bytes = 0
|         preload
|           indexes
|             num = 0
|             totalMillis = 0
|           docs
|             num = 0
|             totalMillis = 0
|     opcountersRepl
|       query = 0
|       command = 0
|       delete = 0
|       getmore = 0
|       update = 0
|       insert = 0
|     opcounters
|       query = 19
|       command = 4
|       delete = 0
|       getmore = 0
|       update = 0
|       insert = 1
|     network
|       bytesOut = 2999
|       bytesIn = 65
|       numRequests = 1
|     indexCounters
|       misses = 0
|       missRatio = 0.0
|       hits = 2
|       resets = 0
|       accesses = 2
|     locks
|       .
|         timeLockedMicros
|           W = 20351
|           R = 3129
|         timeAcquiringMicros
|           W = 447
|           R = 1741
|       local
|         timeLockedMicros
|           w = 0
|           r = 1819
|         timeAcquiringMicros
|           w = 0
|           r = 603
|       admin
|         timeLockedMicros
|           w = 0
|           r = 398
|         timeAcquiringMicros
|           w = 0
|           r = 19
|     asserts
|       user = 0
|       rollovers = 0
|       msg = 0
|       regular = 0
|       warning = 0
|     connections
|       totalCreated = 5
|       available = 838858
|       current = 2
|     writeBacksQueued = false
|     pid = 26
|     version = 2.6.10
|     localTime = 1688637440506
|     uptimeMillis = 565071
|     host = victim-1:6421
|     uptimeEstimate = 561.0
|     backgroundFlushing
|       average_ms = 0.22222222222222
|       last_finished = 1688637415448
|       last_ms = 0
|       flushes = 9
|_      total_ms = 2
41288/tcp open  memcached Memcached
55413/tcp open  ftp       vsftpd 3.0.3
MAC Address: 02:42:C0:8B:81:03 (Unknown)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 2.6.32 (96%), Linux 3.2 - 4.9 (96%), Linux 2.6.32 - 3.10 (96%), Linux 3.4 - 3.10 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), Synology DiskStation Manager 5.2-5644 (94%), Netgear RAIDiator 4.2.28 (94%), Linux 2.6.32 - 2.6.35 (94%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 1 hop
Service Info: OS: Unix

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 18.38 seconds
```
