
***
###### `Title :- Maltego`
###### `Created on :- 2023-08-23 - 19:14`
###### `Created by:- Prem J`
***

- [Maltego](https://www.maltego.com/) is an application that blends mind-mapping with OSINT.
- In general, you would start with a domain name, company name, person’s name, email address, etc. Then you can let this piece of information go through various transforms.
- In Maltego’s terminology, a **transform** is a piece of code that would query an API to retrieve information related to a specific entity

![[Pasted image 20230823191825.png]]

One way to achieve this on Maltego is to right-click on the “DNS Name” `cafe.thmredteam.com` and choose:

1. Standard Transforms
2. Resolve to IP
3. To IP Address (DNS)

After executing this transform, we would get one or more IP addresses, as shown below.
![[Pasted image 20230823191856.png]]

Then we can choose to apply another transform for one of the IP addresses. Consider the following transform:

1. DNS from IP
2. To DNS Name from passive DNS (Robtex)

This transform will populate our graph with new DNS names. With a couple more clicks, you can get the location of the IP address, and so on. The result might be similar to the image below.

![[Pasted image 20230823191959.png]]

- This is just a GUI version of whois and nslookup mixed version

![[Pasted image 20230823192055.png]]