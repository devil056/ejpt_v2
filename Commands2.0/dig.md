#### dig

It is a DNS lookup utility

```
└─$ dig -h
Usage:  dig [@global-server] [domain] [q-type] [q-class] {q-opt}
            {global-d-opt} host [@local-server] {local-d-opt}
            [ host [@local-server] {local-d-opt} [...]]
Where:  domain    is in the Domain Name System
        q-class  is one of (in,hs,ch,...) [default: in]
        q-type   is one of (a,any,mx,ns,soa,hinfo,axfr,txt,...) [default:a]
                 (Use ixfr=version for type ixfr)
        q-opt    is one of:
                 -4                  (use IPv4 query transport only)
                 -6                  (use IPv6 query transport only)
                 -b address[#port]   (bind to source address/port)
                 -c class            (specify query class)
                 -f filename         (batch mode)
                 -k keyfile          (specify tsig key file)
                 -m                  (enable memory usage debugging)
                 -p port             (specify port number)
                 -q name             (specify query name)
                 -r                  (do not read ~/.digrc)
                 -t type             (specify query type)
                 -u                  (display times in usec instead of msec)
                 -x dot-notation     (shortcut for reverse lookups)
                 -y [hmac:]name:key  (specify named base64 tsig key)
        d-opt    is of the form +keyword[=value], where keyword is:
                 +[no]aaflag         (Set AA flag in query (+[no]aaflag))
                 +[no]aaonly         (Set AA flag in query (+[no]aaflag))
                 +[no]additional     (Control display of additional section)
                 +[no]adflag         (Set AD flag in query (default on))
                 +[no]all            (Set or clear all display flags)
                 +[no]answer         (Control display of answer section)
                 +[no]authority      (Control display of authority section)
                 +[no]badcookie      (Retry BADCOOKIE responses)
                 +[no]besteffort     (Try to parse even illegal messages)
                 +bufsize[=###]      (Set EDNS0 Max UDP packet size)
                 +[no]cdflag         (Set checking disabled flag in query)
                 +[no]class          (Control display of class in records)
                 +[no]cmd            (Control display of command line -
                                      global option)
                 +[no]comments       (Control display of packet header
                                      and section name comments)
                 +[no]cookie         (Add a COOKIE option to the request)
                 +[no]crypto         (Control display of cryptographic
                                      fields in records)
                 +[no]defname        (Use search list (+[no]search))
                 +[no]dns64prefix    (Get the DNS64 prefixes from ipv4only.arpa)
                 +[no]dnssec         (Request DNSSEC records)
                 +domain=###         (Set default domainname)
                 +[no]edns[=###]     (Set EDNS version) [0]
                 +ednsflags=###      (Set EDNS flag bits)
                 +[no]ednsnegotiation (Set EDNS version negotiation)
                 +ednsopt=###[:value] (Send specified EDNS option)
                 +noednsopt          (Clear list of +ednsopt options)
                 +[no]expandaaaa     (Expand AAAA records)
                 +[no]expire         (Request time to expire)
                 +[no]fail           (Don't try next server on SERVFAIL)
                 +[no]header-only    (Send query without a question section)
                 +[no]https[=###]    (DNS-over-HTTPS mode) [/]
                 +[no]https-get      (Use GET instead of default POST method while using HTTPS)
                 +[no]http-plain[=###]    (DNS over plain HTTP mode) [/]
                 +[no]https-plain-get      (Use GET instead of default POST method while using plain HTTP)
                 +[no]identify       (ID responders in short answers)
                 +[no]idnin          (Parse IDN names [default=on on tty])
                 +[no]idnout         (Convert IDN response [default=on on tty])
                 +[no]ignore         (Don't revert to TCP for TC responses.)
                 +[no]keepalive      (Request EDNS TCP keepalive)
                 +[no]keepopen       (Keep the TCP socket open between queries)
                 +[no]multiline      (Print records in an expanded format)
                 +ndots=###          (Set search NDOTS value)
                 +[no]nsid           (Request Name Server ID)
                 +[no]nssearch       (Search all authoritative nameservers)
                 +[no]onesoa         (AXFR prints only one soa record)
                 +[no]opcode=###     (Set the opcode of the request)
                 +padding=###        (Set padding block size [0])
                 +qid=###            (Specify the query ID to use when sending queries)
                 +[no]qr             (Print question before sending)
                 +[no]question       (Control display of question section)
                 +[no]raflag         (Set RA flag in query (+[no]raflag))
                 +[no]rdflag         (Recursive mode (+[no]recurse))
                 +[no]recurse        (Recursive mode (+[no]rdflag))
                 +retry=###          (Set number of UDP retries) [2]
                 +[no]rrcomments     (Control display of per-record comments)
                 +[no]search         (Set whether to use searchlist)
                 +[no]short          (Display nothing except short
                                      form of answers - global option)
                 +[no]showbadcookie  (Show BADCOOKIE message)
                 +[no]showsearch     (Search with intermediate results)
                 +[no]split=##       (Split hex/base64 fields into chunks)
                 +[no]stats          (Control display of statistics)
                 +subnet=addr        (Set edns-client-subnet option)
                 +[no]tcflag         (Set TC flag in query (+[no]tcflag))
                 +[no]tcp            (TCP mode (+[no]vc))
                 +timeout=###        (Set query timeout) [5]
                 +[no]tls            (DNS-over-TLS mode)
                 +[no]tls-ca[=file]  (Enable remote server's TLS certificate validation)
                 +[no]tls-hostname=hostname (Explicitly set the expected TLS hostname)
                 +[no]tls-certfile=file (Load client TLS certificate chain from file)
                 +[no]tls-keyfile=file (Load client TLS private key from file)
                 +[no]trace          (Trace delegation down from root [+dnssec])
                 +tries=###          (Set number of UDP attempts) [3]
                 +[no]ttlid          (Control display of ttls in records)
                 +[no]ttlunits       (Display TTLs in human-readable units)
                 +[no]unknownformat  (Print RDATA in RFC 3597 "unknown" format)
                 +[no]vc             (TCP mode (+[no]tcp))
                 +[no]yaml           (Present the results as YAML)
                 +[no]zflag          (Set Z flag in query)
        global d-opts and servers (before host name) affect all queries.
        local d-opts and servers (after host name) affect only that lookup.
        -h                           (print help and exit)
        -v                           (print version and exit)
```

```
dig axfr @<name-server> <target-url>
```
