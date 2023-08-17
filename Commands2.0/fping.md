#### fping

This is another version of the ping command.

Command Usage: 
We need to specify the Interface and the range of IP addresses to be generated.

```
└─$ fping -I eth0 -g 10.0.2.0/24 2>/dev/null   
10.0.2.1 is alive
10.0.2.2 is alive
10.0.2.3 is alive
10.0.2.5 is alive
10.0.2.4 is unreachable
10.0.2.6 is unreachable
10.0.2.7 is unreachable
10.0.2.8 is unreachable
10.0.2.9 is unreachable
10.0.2.10 is unreachable
10.0.2.11 is unreachable
```

And incase if we are not using the 2> we will be seeing the error messages as well. -a is for the devices that are active currently.

```
└─$ fping -I eth0 -g 10.0.2.0/24 2>/dev/null -a          
10.0.2.1
10.0.2.2
10.0.2.3
10.0.2.5

```