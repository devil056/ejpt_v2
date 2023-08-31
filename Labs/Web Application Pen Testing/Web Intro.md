What is a website?

They are just files on a server which we are accessing generally through a browser. The most common files that we see regulary are HTML, CSS and JS. These files are served on localhost using the Apache, Tomcat, Windows IIS and NGINX. In case of the off premise hosting we have vendors like AWS, Wordpress, Azure, Firebase, Digital Ocean, Cloudflare etc.


HTTP Protocol:
We use browsers to connect to the servers and pull these pages on to the local machine and in order to do that we use the following
- Headers - this is used to specify the method to perform and the server on which the method to be executed and the folder from where the data need to be retrieved along with the browser version so that the output is modified accordingly the last thing is the character set to be used as well specified.
- Requests
- Response - Status code and content
- Browsers
- Sessions
- HTTPS


- Client
- Interacts with server
	- Methods
		- GET
		- HEAD
		- POST
		- PUT
		- DELETE
		- CONNECT
		- OPTIONS
		- TRACE
		- PATCH
- User agent

- Response
	- Server
	- Sends Resources
	- Status Codes
		- 200
		- 302
		- 404
		- etc

Lab details: 
- ping demo.ine.local
- As we get the positive response we can open a browser and scan visit the portal we see the file extension php which means it is used by Apache
- Once we run an nmap scan we can see that the port 80 and 3306 are open
- Launch the wireshark and select the interface and try to login using the default credentials
- We can use the curl to pull over the copy of the webpage
- ping demossl.ine.local it has got both the port 80 and port 443 open along with the database port 3306
- use dirb and explore