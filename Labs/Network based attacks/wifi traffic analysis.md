A WiFi traffic capture is provided in the lab. Analyze the traffic using Wireshark and answer the following questions:

1. What is the name of the Open (No Security) SSID present in the packet dump?
2. The SSID 'Home_Network' is operating on which channel?
3. Which security mechanism is configured for SSID 'LazyArtists'? Your options are: OPEN, WPA-PSK, WPA2-PSK.
4. Is WiFi Protected Setup (WPS) enabled on SSID 'Amazon Wood'?  State Yes or No.
5. What is the total count of packets which were either transmitted or received by the device with MAC e8:de:27:16:87:18?
6. What is the MAC address of the station which exchanged data packets with SSID 'SecurityTube_Open'?
7. From the last question, we know that a station was connected to SSID 'SecurityTube_Open'. Provide TSF timestamp of the association response sent from the access point to this station.
---
1.
```
(wlan.fc.type_subtype == 0x0008) && !(wlan.wfa.ie.wpa.version ==1) && !(wlan.tag.number==48)
```

2.
```
wlan contains Home_Network
```

3.
```
wlan contains LazyArtists
```

4.
```
(wlan.ssid contains Amazon) && (wlan.fc.type_subtype==0x0008)
```

5.
```
(wlan.ta==e8:de:27:16:87:18) || (wlan.ra==e8:de:27:16:87:18)
```

6.
```
((wlan.bssid==e8:de:27:16:87:18)&&(wlan.fc.type_subtype==0x0020))
```

7.
```
(((wlan.bssid==e8:de:27:16:87:18))&&(wlan.addr==5c:51:88:31:a0:3b))&&(wlan.fc.type_subtype==0x0001)
```

