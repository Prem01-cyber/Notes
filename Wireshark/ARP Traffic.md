
***
###### `Title :- ARP Traffic`
###### `Created on :- 2023-08-19 - 07:41`
###### `Created by:- Prem J`
***

- ARP or Address Resolution Protocol is a **Layer 2** protocol that is used to connect IP Addresses with MAC Addresses
- They contain request and response messages. To identify packets the message header will contain one of two operation codes ***Request (1)*** and ***Response (2)***

![[Pasted image 20230819074756.png]]

- most devices will identify themselves or Wireshark will identify it such as Intel_78
- You need to enable a setting within Wireshark however to resolve physical addresses.
- To enable this feature, navigate to View > Name Resolution > Ensure that Resolve Physical Addresses is checked.

![[Pasted image 20230819093550.png]]

#### `ARP Request Packets -->`

![[Pasted image 20230819093614.png]]

Opcode - Operation code - tells us if the packet is request or reply

#### `ARP Reply Packets -->`

![[Pasted image 20230819093707.png]]

We get additionally Sender MAC and IP addresses with it.