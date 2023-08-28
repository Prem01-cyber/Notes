
***
###### `Title :- Log Ingestion`
###### `Created on :- 2023-08-24 - 22:36`
###### `Created by:- Prem J`
***

![[Pasted image 20230824223940.png]]

- All the logs have wealthy information and can help in identifying security issues. 
- Each SIEM has it's own way of ingesting logs.

1) ***Agent / Forwarder***: These SIEM solutions provide a lightweight tool called an agent (forwarder by Splunk) that gets installed in the Endpoint. It is configured to capture all the important logs and send them to the SIEM server.  

2) ***Syslog***: Syslog is a widely used protocol to collect data from various systems like web servers, databases, etc., are sent real-time data to the centralized destination.

3) ***Manual Upload***: Some SIEM solutions, like Splunk, ELK, etc., allow users to ingest offline data for quick analysis. Once the data is ingested, it is normalized and made available for analysis.

4) ***Port-Forwarding***: SIEM solutions can also be configured to listen on a certain port, and then the endpoints forward the data to the SIEM instance on the listening port.

![[Pasted image 20230824224113.png]]

>[!Note]
>Splunk is a SIEM tool

