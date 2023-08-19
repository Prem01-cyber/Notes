
***
###### `Title :- DNS Traffic`
###### `Created on :- 2023-08-19 - 10:20`
###### `Created by:- Prem J`
***

- DNS or Domain Name Service protocol is used to resolves names with IP addresses.
- Initial stages of how websites work

- There are a couple of things outlined below that you should keep in the back of your mind when analyzing DNS packets.
	
	- Query-Response
	- DNS-Servers Only
	- UDP

![[Pasted image 20230819102221.png]]

#### `DNS Query -->`

![[Pasted image 20230819102237.png]]

Query is originating from UDP port 53, if it was TCP 53 then its suspicious.

>[!Tip]
>When analyzing DNS packets you really need to understand your environment and whether or not the traffic would be considered normal within your environment.

#### `DNS Response -->`

![[Pasted image 20230819102433.png]]

Answer to the queried information