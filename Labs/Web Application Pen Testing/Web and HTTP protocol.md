Lab details:

- This is an nteractive GUI based lab as we can see that the target IP is our adjacent in the very same network on eth1
- Open a browser and try to check the IP on both http and https which pulls up the vulnerable application
- Sending a get request using `curl -X GET {targetIP}` and head method as well it failed so we will run with `curl -l {targetIP}` its not -l its -I
- We got a php session ID and path information
- Try checking the `curl -X OPTIONS {targetIP}` along with the verbosity enable flag we get a list of options that are allowed on the IP address
- Incase if we are to try other method that is not allowed we get a code 405 stating that the method is not allowed
- Try the same we just did with the login.php at the end of the targetIP with curl
- Let us pass information along with the command `curl -X PUT {targetIP}/login.php -d "name={username}&password={userPassword}"` once the command is executed we get the code 302 and we are able to access the index.php file now
- Let us check the posts.php page now for the OPTIONS
- Checking for the uploads methods now , we got the 404 not found back from the server
- As per the command we are able to upload a file into the uploads directory so in order to upload a file we can use the same command as follows `curl {targetIP}/uploads/ --upload-file {pathToFile}`
- Try to delete as well `curl -X DELETE {targetIP}/uploads/{file}`
- Downloading a file using curl `curl {targetIP}/uploads/{file}` 

With burp

- Open burpsuite and start a temporary project
- Actions > Send to repeater
- We can change the options in the request instead of GET change it to HEAD,POST,OPTIONS and all
- Try loggin in using the credentials we can see a 302