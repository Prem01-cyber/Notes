
***
###### `Title :- HTTP Traffic`
###### `Created on :- 2023-08-19 - 10:27`
###### `Created by:- Prem J`
***

HTTP or Hypertext Transfer Protocol is a commonly used port for the world wide web and used by some websites

Its encrypted counterpart HTTPS is more common

![[Pasted image 20230819102818.png]]

In HTTP data stream is not encrypted, so the information is carried in plain-text
From this packet we can identify some very important information like the host, user-agent, requested URI, and response

![[Pasted image 20230819102941.png]]

We can take advantage of wireshark and digest all this data to help and organise it for further future analysis

![[Pasted image 20230819103039.png]]

The next feature in Wireshark we will look at is the Export HTTP Object. This feature will allow us to organize all requested URIs in the capture. To use Export HTTP Object navigate to file > Export Objects > HTTP.

![[Pasted image 20230819103052.png]]

Above is the listing of endpoints gathered during the traffic capture