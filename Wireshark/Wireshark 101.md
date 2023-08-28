
***
###### `Title :- Wireshark 101`
###### `Created on :- 2023-08-19 - 01:48`
###### `Created by:- Prem J`
***

#### `Installation -->`

- [Installation](https://www.wireshark.org/download.html)
- [Documentation](https://www.wireshark.org/docs/)

#### `Overview -->`

- tool used to sniff network traffic over a particular interface.
- We can further filter the results based on our requirements.

![[Pasted image 20230819015755.png]]

![[Pasted image 20230819015833.png]]

- Wireshark gives us important info about each packet. For instance,
	- Packet Number
	- Time
	- Source
	- Destination
	- Protocol
	- Length
	- Packet Info

- Along with quick packet information, Wireshark also color codes packets in order of danger level as well as protocol to be able to quickly spot anomalies and protocols in captures.

![[Pasted image 20230819015939.png]]

- Before going into detail about how to analyze each protocol in a PCAP we need to understand the ways to gather a PCAP file.

>[!Note]
>these wireshark files as save as PCAP files. 

- The basic steps to gather a PCAP in Wireshark itself can be simple however bringing into traffic can both the hard part as well as the fun part, this can include: taps, port mirroring, MAC floods, ARP Poisoning.

#### `Network Taps -->`

- Are a physical implant win which you physically tap between a cable
- These techniques are commonly used by Threat Hunting/DFIR teams and read teams in an engagement to sniff and capture packets.
- There are two primary means of tapping a wire, hardware device implant to intercept and planting a network tap between two inline or two network devices

![[7.gif]]

![[Pasted image 20230819020530.jpg]]

#### `MAC Floods -->`

- Mac flooding is commonly used to fill the CAM table and cause a reset
- This is often used by red teams as a way of actively sniffing packets

#### `ARP Poisioning -->`

- By ARP poisoning you can redirect the traffic from the host to the machine you're monitoring from.
- This technique will not stress network equipment like MAC Flooding however should still be used with caution and only if other techniques like network taps are unavailable.
- Used by red teams

- Combining this knowledge with the previous knowledge of capturing traffic will allow to proactively monitor and collect live packet captures from scratch.