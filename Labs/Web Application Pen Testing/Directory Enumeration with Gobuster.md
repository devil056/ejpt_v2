Lab details:
- It is a command line tool
- And the lab env is Interactibe GUI based kali machine with the target IP adjacent to our current IP on the network
- Let us find the directories on the webapp and the command is `gobuster dir --help` to get all the options that are available
- Incase if we are filtering based on the status codes we can do so using the -b flag and list the codes you wish to silence
- Once we got the list we can use the ones that we are interested in like php xml or txt files you can use -r at the end so we can even enumerate the directories and their content as well