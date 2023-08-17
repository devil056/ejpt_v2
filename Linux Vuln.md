### Linux Vulnerabilities

- Linux is a free and open source operating system that is comprised of the Linux Kernel, which was developed by Linus Torvalds, and the GNU toolkit, which is a collection of software and utilities that was started and developed by by Richard Stallman.
- This combination of open source software is what makes up the Linux OS as a whole, and it is commonly referred to as GNU/Linux
- Linux has various use cases, however, it is typically deployed as a server operating system. For this reason, there are specific services and protocols that will typically be found running on a Linux server.
- These services provide an attacker with an access vector that they can utilize to gain access to a target host.
- Having a good understanding of what these services are, how they work and their potential vulnerabilities is a vitally important skill to have as a penetration tester.

Protocol | Ports | Purpose
:--: | :--: | :--:
Apache web server | PCT ports 80/443 | Free and open source cross-platform web server released under the Apache license 2.0 Apache accounts for over 80% of web servers globally
SSH (Secure Shell) | TCP ports 22 | SSH is a cryptographic remote access potocol that is used to remotely access and control systems over and unsecured network. SSH was developed as a secure successor to telnet
FTP (File Transfer Protocol) | TCP port 21 | FTP is a protocol that uses TCP port 21 and is used to facilitate file sharing between a server and client/clients and vice versa
SAMBA| TCP Port 445 | Samba is the linux implementation of the SMB, and allows Windows systems to access Linux shares and devices

