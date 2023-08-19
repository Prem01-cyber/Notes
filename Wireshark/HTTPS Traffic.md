
***
###### `Title :- HTTPS Traffic`
###### `Created on :- 2023-08-19 - 10:47`
###### `Created by:- Prem J`
***

HTTPS or Hyper Text Transfer Protocol Secure can be one of the most annoying protocols to understand from a packet analysis perspective. It can be confusing

#### `Overview -->`

Before sending encrypted information the client and server need to agree upon various steps in order to make a secure tunnel.

1. Client and server agree on a protocol version
2. Client and server select a cryptographic algorithm
3. The client and server can authenticate to each other; this step is optional
4. Creates a secure tunnel with a public key

So in order to communicate a handshake should be performed between client and server.

`Client Hello` packet showing the SSLv2 Record Layer, Handshake Type, and SSL Version.

![[Pasted image 20230819104933.png]]

`Server Hello` packet sending similar information as the Client Hello packet however this time it includes session details and SSL certificate information

![[Pasted image 20230819105020.png]]

`Client Key exchange` packet, this part of the handshake will determine the public key to use to encrypt further messages between the Client and Server

![[Pasted image 20230819105048.png]]

`Server Confiramation` of the public key and creates a tunnel to communicate

![[Pasted image 20230819105118.png]]

past this everything is encrypted, we would require the secret key inorder to decrypt the data stream being sent between two hosts

![[Pasted image 20230819105158.png]]

>[!Note]
>We can confirm from the packet details that the Application Data is encrypted. You can use an RSA key in Wireshark in order to view the data unencrypted. In order to load an RSA key navigate to Edit > Preferences > Protocols > TLS >  [+] . If you are using an older version of Wireshark then this will be SSL instead of TLS. You will need to fill in the various sections on the menu with the following preferences:

![[Pasted image 20230819105301.png]]