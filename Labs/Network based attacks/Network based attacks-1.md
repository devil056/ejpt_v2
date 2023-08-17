Network based attacks is the one which deals with network and network services. It is not something that is related to the operating system. It usually includes the packets that are travelling through network when the services that uses the network.
Network services:
- ARP
- DHCP
- SMB
- FTP
- Telnet
- SSH

**Man in the middle attacks**

In order to perform the Man in the middle attacks we use a tool called Wireshark. It is one of the most used tool when it comes to mitm attacks. Let us assume we are on a network with multiple systems and out of them there are two system interacting with one another and we will be able to hear their data traffic. Considering the way the network is set now a days the data that is being transmitted is sent in packets and it will be enclosed in multiple layers with meta data. And once the packet is sent it might be hitting multiple end points (reach other systems on the same network) or all the end points in some cases. So in order to track that traffic we might have to connect to the network using span port which will enable us to track all the traffic on network or poison the network and the easist way to do it is arp-poisoning. In the earlier days where all the systems where connected to the hub we would have gotten access to listen all the traffic with ease as now we have moved on to using switches the scenario has changed so we either of the approaches.

Arp-poisoning : It is technique where we broadcast packets on network indicating that our machine IP address is something else (in this case the target we are trying to hack) and capture the packets that are being sent to the target IP address.

In insider network the packets are bound to an IP address but they are routed within the network based on the MAC addresses. In case if we are to poison the network we will be able get the packets that are sent to other network to us.

Promiscuios mode : In this mode we will be connected as just another users and track all the packets that are in the network and later on filter the packets using the filters that we can set and separate the packets which we want and analyze them. Even though the packets are not really for us using this method we will be able to track all the packets.

In both the modes of capture options the first thing we need to do is capture the data in packets that is being routed in the network. In order to do that we can use a GUI tool called `Wireshark` and we also do have a Non GUI version called `tshark` So we can start off the wireshark like any other application and the first screen that we see is a list of interfaces. We need to select the interface using which we wish to capture the data and press the capture button at the top left corner of the screen and the capture packets will be started. In order to get data flow in the application you can try to do some google search or run the nmap ping scan and so on. All that data will be captured in the wireshark. One you think that you have enough amount of data we can stop the capture and analyze. Once the capturing of packets is done we can get a clear picture of the hierarchy of protocols and the end points and so on in wireshark.