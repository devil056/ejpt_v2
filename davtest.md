### davtest

It is pre installed on Kali linux.
This program attempts to exploit WebDAV enabled servers by:

- attempting to create a new directory (MKCOL)
- attempting to put test files of various programming langauges (PUT)
- optionally attempt to put files with .txt extension, then move to executable (MOVE)
- check if files executed or were uploaded properly
- optionally upload a backdoor/shell file for languages which execute

Additionally, this can be used to put an arbitrary file to remote systems.

```
root@attackdefense:~# davtest

ERROR: Missing -url

/usr/bin/davtest -url <url> [options]

 -auth+ 	Authorization (user:password)
 -cleanup	delete everything uploaded when done
 -directory+	postfix portion of directory to create
 -debug+	DAV debug level 1-3 (2 & 3 log req/resp to /tmp/perldav_debug.txt)
 -move		PUT text files then MOVE to executable
 -nocreate 	don't create a directory
 -quiet	 	only print out summary
 -rand+ 	use this instead of a random string for filenames
 -sendbd+	send backdoors:
			auto - for any succeeded test
			ext - extension matching file name(s) in backdoors/ dir
 -uploadfile+	upload this file (requires -uploadloc)
 -uploadloc+	upload file to this location/name (requires -uploadfile)
 -url+		url of DAV location

Example: /usr/bin/davtest -url http://localhost/davdir
```

#### basic command

```
davtest -url <url> -auth (user:password)
```

