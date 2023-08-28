
***
###### `Title :- SNMP`
###### `Created on :- 2023-05-22 - 20:42`
###### `Created by:- Prem J`
***
#### `Brief -->`

- Netowork Mangement Protocol for monitoring and managing network devices.
- The current version is **SNMPv3** (More security and more complexity).

#### `Used For -->`

- Monitoring newtwork devices like routers switches IoT devices and many other devices that can also be queried and controlled using standard protocols.

#### `How does it work -->`

- SNMP collects information from a large fleet of network  devices. It relies of client server application model. SNMP also sends control commands over **UDP 161**.
- The client can set a specific value in the device and change options and settings with these commands by which client can control the network devices.
- SNMP sends **traps**[^1] over **UDP 162**.
- To exachange data from SNMP client and server each object must have **unique addresses** on both sides.

###### `MIB (Management Information Base)`

- To ensure that SNMP access works acroos manufacturers with different client-server combinations MIB wascreated. It is an Independent format used to store device information.
- It is a **Text File** with **Abstract Syntax Notation One (ASN.1)** ASCII text format in which all queryable SNMP objects (routers IoT ...e.t.c) of a device are listed in a standardized **tree hierearchy**.
- In the hierarchy namespace[^2] it contains atleast one **Object Identifier(OID)**[^3] , necessary unique address and name, information about the type, access rights and description of the object.

>[!info]
>The MIBs do not contain data, but they explain where to find which information and what it looks like, which returns values for the specific OID, or which data type is used.

#### `Advantages -->`



#### `Alternatives -->`

- There are 3 versions of SNMP SNMPv1, SNMPv2 or v2c (c indicated community-based SNMP), SNMPv3. These are less complex when compared to the current version.

>[!info]
>Many organisations still use SNMPv2, as the transition to SNMPv3 can be very complex

#### `Configuration Files -->`

- The default configuration of the SNMP daemon defines the basic settings for the service, which include the IP addresses, ports, MIB, OIDs, authentication, and community strings.

> 	SNMP Daemon Config --> /etc/snap/snmpd.conf

- There are some dangerous configuaration that need to be known.

**rwuser noauth** --> Provides full access to the full OID tree without authentication
**rwcommunity "community string"  "ip addr"**--> Provides access to full OID tree regardless of where the requests were sent from (ip4)
**rwcommunity6 "community string" "ip addr"** --> Same as previous but ip6

#### `Commands -->`



#### `Vulnerabilities -->`

- **SNMPv1** has all the function as SNMPv3 but **doen't have built-in authetication** and does **not support encryption**, Allowing anyone to access the network and modifiy the data.
- **SNMPv2** or **v2c** is on par with SNMPv1 and has been extended with additional functions from the part-based no longer in use. However, a significant problem with the initial execution of the SNMP protocol is that the  **Community string**[^4] that provides security is only transmitted in plain text, meaning it has **no built-in encryption**
- As many organisations still use SNMPv2, this causes a great deal of concern to Administrators and create some problems that are keen to avoid .i.e Lack of encryption.

#### `Remedies -->`



#### `Footprinting Walkthrough -->`

- 
- **snmp-check** a basic tool to identify information on SNMP devices. It supports enumeration of hostname, devices, harware and storage information, contact, description etc.

>	snmp-check "ip addr"

- We start footprinting the service using tools like **snmpwalk**, **onesixtyone** and **braa**.
- we use snmpwalk to query the OIDs with their information

>	snmpwalk -v2c -c public 10.129.14.128

- Once we get to know that there are **misconfiguration** in the SNMP server we can know the community string by brute forcing the service using onesixtyone.
- we use onesixtyone to brute force the names of the community strings.

>	onesixtyone -c /opt/useful/Seclists/Discovery/SNMP/snmp.txt 10.129.14.128

>[!info]
>Often, when certain community strings are bound to specific IP addresses, they are named with the hostname of the host, and sometimes even symbols are added to these names to make them more challenging to identify. However, if we imagine an extensive network with over 100 different servers managed using SNMP, the labels, in that case, will have some pattern to them. Therefore, we can use different rules to guess them. We can use the tool [crunch](https://secf00tprint.github.io/blog/passwords/crunch/advanced/en) to create custom wordlists. Creating custom wordlists is not an essential part of this module, but more details can be found in the module [Cracking Passwords With Hashcat](https://hashcat.net/hashcat/).Once we know a community string, we can use it with **braa** to brute-force the individual OIDs and enumerate the information behind them.

- Brute forcing OIDs with braa

>	braa "community string"@"ip addr":.1.3.3.* #syntax --> no idea what this is
>	braa public@10.1129.14.128:.1.3.6.*

- .1.3.6.* represents the OIDs

#### `Summary -->`

Once again, we would like to point out that the independent configuration of the SNMP service will bring us a great variety of different experiences that no tutorial can replace. Therefore, we highly recommend setting up a VM with SNMP, experimenting with it, and trying different configurations. SNMP can be a boon for an I.T. systems administrator as well as a curse for Security analysts and managers alike.


[^1]: Traps are the data packets that are sent from SNMP server to client when a device is configured accordingly from client (if a specific event occurs on the server side.)
[^2]: Namespace
[^3]: OID are the a sequence of numbers uniquely identifies each node, allowing the node's position in the trees to be determined. Many nodes contain the reference to the below nodes for example [Object Identifier registry](https://www.alvestrand.no/objectid/)
[^4]:  Community Strings can be seen as passwords that are used to determine weather a requested information can be viewed or not.
