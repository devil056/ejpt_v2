### Servers and services
`ris:PriceTag3` [[Recon]] [[Active Recon]]
`ris:Tools`

#### Common services and their port numbers:

> What is a server and service ?
> A server is basically a system placed at a remote location and server something. It provides a specialized service that can be utilized by other devices connected to the network rather than giving access to have the service on all devices. This way it keeps it centralized and keep a track of all the users and the data traffic aswell.

We often refer them in context of office management terms where various resources like emails, file sharing, messaging and other services are hosted by one specialised computer and it can be running any operating system. Based on how we do set up any computer is capable of being a server we use different operating systems for servers based on licensing,security and streamlining operations.For cybersecurity professionals it all comes down to how well it was managed as we still end up facing issues. Servers by nature are supposed to work remotely so that multiple systems can access different features based on the priviledges provided to the users. And the services running on the server should open / listen on ports and accept connections remotely. So we do open up the ports which by default not the problem. The problem comes with the bugs,flaws,features,misconfiguarations of system which might be exploited by the hackers.

The most common services that we currently use are

Service | Port No.
:--: | :--:
SMB | 445/139
FTP | 21
SSH | 22
HTTP |80
SQL |3306 / 1433



#### SMB
It is a service windows implementation of a file sharing service. **Server Message Bloack**
- SMB (Server Message Block) is a network file sharing protocol that is used to facilitate the sharing of files and peripherals (printers and so on) between computers on a LAN (local network).
- SMB uses port 445(TCP). However originally SMB ran on top of NetBIOS using port 139.
- SAMBA is the open source linux implementation of SMB, and allows windows systems to access the linux shares and devices.
- SMB protocol utilizes two levels of authentication
	- User authentication
	- Share authentication
- User authentication - users must provide a username and password in order to authenticate with the SMB server in order to access a share
- Share authentication - users must provide a password in order to access restricted share.
- We can utilize auxiliary modules to enumerate the SMB version, shares, users and perform a brute force attack in order to identify users and passwords.

Note: Both of these authetication levels utilize a challenge response authetication system.

#### FTP
file transfer protocol
- FTP is a protocol that uses TCP port 21 and is used to facilitate file sharing between a server and client/clients
- It is also frequently used as a means of transferring files to and from the directory of a web server
- We can use multiple auxiliary modules to enumerate information as well as perform brute force attacks on targets running an FTP server
- FTP authentication utilizes a username and password combination, however, in some cases an improperly configured FTP server can be logged into anonymously.

#### SSH

- SSH (Secure Shell) is a remote administration protocol that offers encryption and is the successor to Telnet.
- It is typically used for remote access to servers and systems
- SSH uses TCP port 22 by default, however, like other services, it can be configured to use any other open TCP port
- We can utilize auxiliary modules to enumerate the version of SSH running on the target as well as perform brute force attacks to identify passwords that can consequently provide us remote access to a target.

#### HTTP

Webserver enumeration
- A web server is a software that is used to serve website data on the web.
- Web servers utilize HTTP (Hypertext Transfer Protocol) to facilitate the communication between clients and the web server
- HTTP is an application layer protocol that utilizes TCP port 80 for communication
- We can utilize auxiliary modules to enumerate the web server version, HTTP headers, brute force directories and much more.
- Examples of popular web servers are : Apache, Nginx and Microsoft IIS.

#### SQL
port 3306 --> MySQL
port 1433 --> MsSQL

basic connect

```
mysql -h <target-ip> -u root
```

```
show databases;
```

MySQL:
- MySQL is an open sourec relational database management system based on SQL (Structured Query Language)
- It is typically used to store records, customer data, and is most commonly deployed to store web application data.
- MySQL utilizes TCP port 3306 by default, however like any service it can be hosted on any open TCP port
- We can utilize auxiliary modules to enumerate the version of MySQL, perform brute force attacks to identify passwords, execute SQL queries and much more.

#### SMTP

- SMTP (Simple Mail Transfer Protocol) is a communication protocol that is used for the transmission of email.
- SMTP uses TCP port 25 by default. It can also be configured to run on TCP port 465 and 587.
- We can utilize auxiliary modules to enumerate the version of SMTP as well as user accounts on the target system.