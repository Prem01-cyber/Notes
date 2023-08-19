
***
###### `Title :- TCP Traffic`
###### `Created on :- 2023-08-19 - 10:11`
###### `Created by:- Prem J`
***

- TCP or Transmission Control Protocol handles the delivery of packets including sequencing and errors

![[Pasted image 20230819101738.png]]

- above is a nmap scan over the ports 80 and 443

- A common thing that you will see when analyzing TCP packets is known as the TCP handshake. It includes syn,syn-ack and ack 

![[Pasted image 20230819101850.png]]

#### `SYN Packet -->`

![[Pasted image 20230819101909.png]]

The main thing that we want to look for when looking at a TCP packet is the sequence number and acknowledgement number.

Within Wireshark, we can also see the original sequence number by navigating to edit > preferences > protocols > TCP > relative sequence numbers (uncheck boxes).

![[Pasted image 20230819102012.png]]

![[Pasted image 20230819102021.png]]