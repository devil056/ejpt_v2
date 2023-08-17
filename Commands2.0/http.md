### http

CLI, cURL-like tool for humans

```
usage: http [--json] [--form] [--compress] [--pretty {all,colors,format,none}] [--style STYLE] [--unsorted] [--sorted] [--format-options FORMAT_OPTIONS] [--print WHAT] [--headers] [--body] [--verbose] [--all] [--history-print WHAT] [--stream] [--output FILE]
            [--download] [--continue] [--session SESSION_NAME_OR_PATH | --session-read-only SESSION_NAME_OR_PATH] [--auth USER[:PASS]] [--auth-type {basic,digest}] [--ignore-netrc] [--offline] [--proxy PROTOCOL:PROXY_URL] [--follow] [--max-redirects MAX_REDIRECTS]
            [--max-headers MAX_HEADERS] [--timeout SECONDS] [--check-status] [--path-as-is] [--verify VERIFY] [--ssl {ssl2.3,tls1,tls1.1,tls1.2}] [--ciphers CIPHERS] [--cert CERT] [--cert-key CERT_KEY] [--ignore-stdin] [--help] [--version] [--traceback]
            [--default-scheme DEFAULT_SCHEME] [--debug]
            [METHOD] URL [REQUEST_ITEM ...]
http: error: the following arguments are required: URL
```

#### Basic command

```
http <target-ip>/<URL>
```

