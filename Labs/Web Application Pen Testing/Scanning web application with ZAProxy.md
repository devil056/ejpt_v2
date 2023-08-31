Lab details:

- It is a GUI based lab environment try to find your IP address while the target IP is your adjacent IP address
- Perform a quick nmap scan. We see that the target has port 80 and 3306 open. Check the browser and we can see that bwapp app running on port 80.
- Now launch the zaproxy app
- Click on the manual explore and in the url path to explore use `http://{targetIP}` and the enable to HUD enabled and the browser of your choice
- We will be in a new browser and see new options on the browser appearing as well
- It is matter of crawling as we keep exploring the application new infomation will be added on to the zaproxy application
- From the login.php in post in the zaproxy app we right click and select the include in context > Default context
- A new pop up will be open select the Authentication and choose form based authentication below select the form again from where the pop up was opened again
- Set username parameter as login and password parameter as password
- In the Regex pattern identified in Logged out as Login
- Add users in the below option
- Check if the usermode is enabled or not
- Right click on the default context and we see the attack option select it with the spider, choose user and start the scan
- Now perform an active scan