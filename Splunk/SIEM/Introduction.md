
***
###### `Title :- Introduction`
###### `Created on :- 2023-08-24 - 22:24`
###### `Created by:- Prem J`
***
#### `SIEM -->`

- SIEM stands for **Security Information and Event Management system**.
- It is a tool that collects data from various endpoints network devices across the network and stores them at a centralized place and performs correlation on them.

#### `Network Visiblity through SIEM -->`

![[Pasted image 20230824222752.png]]

- As we know, each network component can have one or more log sources generating different logs
- One example could be setting up Sysmon along with Windows Event logs to have better visibility of Windows Endpoint. We can divide our network log sources into two logical parts:

1. Host-Centric Log Sources -> events that occurred within or related to the host
	- Some log sources that generate host-centric logs are Windows Event logs, Sysmon, Osquery, etc. Some examples of host-centric logs are:
		- A user accessing a file
		- A user attempting to authenticate.
		- A process Execution Activity
		- A process adding/editing/deleting a registry key or value.
		- Powershell execution


2. Network-Centric Log Sources -> logs when two hosts communicate with each other
	- Some network-based protocols are SSH, VPN, HTTP/s, FTP, etc. Examples of such events are:
		- SSH connection
		- A file being accessed via FTP
		- Web traffic
		- A user accessing company's resources through VPN.



![[Pasted image 20230824223304.png]]