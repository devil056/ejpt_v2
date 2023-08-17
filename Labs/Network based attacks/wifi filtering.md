WiFi is everywhere: home, work, airport, hospitals, shopping complexes and even on airplanes :) In these exercises, you will explore how to analyze Wi-Fi traffic using tshark.

Start the lab, locate the PCAP file and use tshark to answer the below questions:

Questions:

Set A:

1. Which command can be used to show only WiFi traffic?
2. Which command can be used only view the deauthentication packets?
3. Which command can be used to only display WPA handshake packets?
4. Which command can be used to only print the SSID and BSSID values for all beacon frames?

**Set B:**

1. What is BSSID of SSID “LazyArtists”?
2. SSID “Home_Network” is operating on which channel?
3. Which two devices received the deauth messages? State the MAC addresses of both.
4. Which device does MAC 5c:51:88:31:a0:3b belongs to? Mention manufacturer and model number of the device.
---
A-1
```
tshark -r WiFi_traffic.pcap -Y 'wlan'
```

A-2
```
tshark -r WiFi_traffic.pcap -Y 'wlan.fc.type_subtype==0x00c'
```

A-3
```
tshark -r WiFi_traffic.pcap -Y 'eapol'
```

A-4
```
tshark -r WiFi_traffic.pcap -Y 'wlan.fc.type_subtype==8' -Tfields -e wlan.ssid -e wlan.bssid
```

B-1
```
tshark -r WiFi_traffic.pcap -Y 'wlan.ssid==LazyArtists' -Tfields -e wlan.bssid
```

B-2
```
tshark -r WiFi_traffic.pcap -Y 'wlan.ssid==Home_Network' -Tfields -e wlan_radio.channel
```

B-3
```
tshark -r WiFi_traffic.pcap -Y 'wlan.fc.type_subtype==0x00c' -Tfields -e wlan.ra
```

B-4
```
tshark -r WiFi_traffic.pcap -Y 'wlan.ta==5c:51:88:31:a0:3b && http' -Tfields -e http.user_agent
```