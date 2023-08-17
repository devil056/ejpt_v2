#### whatweb

```
WhatWeb - Next generation web scanner version 0.5.5.
Developed by Andrew Horton (urbanadventurer) and Brendan Coles (bcoles).
Homepage: https://www.morningstarsecurity.com/research/whatweb

Usage: whatweb [options] <URLs>

TARGET SELECTION:
  <TARGETs>                     Enter URLs, hostnames, IP addresses, filenames or
                                IP ranges in CIDR, x.x.x-x, or x.x.x.x-x.x.x.x
                                format.
  --input-file=FILE, -i         Read targets from a file. You can pipe
                                hostnames or URLs directly with -i /dev/stdin.

TARGET MODIFICATION:
  --url-prefix                  Add a prefix to target URLs.
  --url-suffix                  Add a suffix to target URLs.
  --url-pattern                 Insert the targets into a URL.
                                e.g. example.com/%insert%/robots.txt
```
