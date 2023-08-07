
***
###### `Title :- Subdomain Enumeration`
###### `Created on :- 2023-05-29 - 14:14`
###### `Created by:- Prem J`
***
#### `Brief -->`

- We can perform **active subdomain enumeration** probing the **infrastructure** managed by the target organisation or the 3rd party DNS servers we have previously identified. In this case, the amount of traffic generated can lead to the detection of our reconnaissance activities.
- Remember that this is all done on port 53.

### `Zone Transfer -->`

- it is how a **secondary DNS server** receives information from **primary DNS server** and updates it.
- Master-slave approach is used to maintain DNS servers within a domain.
- Slaves receive updated DNS information from the master DNS.
- Master DNS servers should be configured to enable zone transfer from secondary (slave) DNS servers.

##### `Automated Approach (Zone Trasfer) -->`

- we can use [hackertarget](https://hackertarget.com/zone-transfer/) service to obtain information about zone transfer on a target.
![[zonetransfer.png]]

##### `Manual Approach (Zone Transfer) -->`

##### 1. **Identifying Name Servers :**

- We use **nslookup** to identify the name servers 

>`nslookup -type=NS zonetransfer.me`

>[!Note]
>while using nslookup we can mention the webserver that we want to query with by default it queries 1.1.1.1.

##### 2. **Testing for ANY and AXFR Zone Transfer :**

- Perform the Zone transfer using `-type=any` and `-query=AXFR` parameters

>`nslookup -type=any -query=AXFR zonetransfer.me nsztm1.digi.ninja`

![[Screenshot from 2023-05-29 20-20-39.png]]

- Upon performing the above command we can find something like this indicating the zone transfer.

>[!tip]
>If we manage to perform a successful zone transfer for a domain, there is no need to continue enumerating this particular domain as this will extract all the available information.




- we can use [[Gobuster]] for the subdomain enumeration, which follows brute forcing mechanism.