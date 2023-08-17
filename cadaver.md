### Cadaver

_cadaver_ is a command-line WebDAV client, with support for file upload, download, on-screen display, in-place editing, namespace operations (move/copy), collection creation and deletion, property manipulation, and resource locking.

```
root@attackdefense:~# cadaver -h
Usage: cadaver [OPTIONS] http://hostname[:port]/path
  Port defaults to 80, path defaults to '/'
Options:
  -t, --tolerant            Allow cd/open into non-WebDAV enabled collection.
  -r, --rcfile=FILE         Read script from FILE instead of ~/.cadaverrc.
  -p, --proxy=PROXY[:PORT]  Use proxy host PROXY and optional proxy port PORT.
  -V, --version             Display version information.
  -h, --help                Display this help message.
Please send bug reports and feature requests to <cadaver@webdav.org>
```

