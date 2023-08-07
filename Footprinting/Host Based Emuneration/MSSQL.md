
***
###### `Title :- MSSQL`
###### `Created on :- 2023-05-24 - 22:30`
###### `Created by:- Prem J`
***
#### `Brief -->`

- Microsoft's SQL-based closed-source relational database management system, initially written to run on windows OS.
- Default TCP Port that MSSQL listens on is **1433** 

#### `Used For -->`

- Same function as [[MySQL]].

#### `How does it work -->`

- It is popular among database administrators and developers when building applications that run on Microsoft's .NET framework due to its strong native support for .NET. 
- **SQL Server Management Studio** is installed on server for initial configuration and long term management of databases.

>[!Note]
>SSMS is a client-side application and it can be installed and used on any system an admin or developer is planning to manage the database from.

- There are many other clients that we can use to access a database

**mssql-cli**
**SQL Server Powershell**
**HediSQL**
**SQLPro**
**[Impacker's mssqlclient.py](https://github.com/fortra/impacket/blob/master/examples/mssqlclient.py)** --> Many find this very helpful

- MSSQL has default system databases that can help us understand the **structure of all the databases** that may be hosted on a target server. 

**master** --> Tracks all info of SQL server instance
**model** --> Template database acts as a structure for the newly created databases
**msdb** --> SQL Server agent uses this to schedule jobs and alerts
**tempdb** --> Stores temporary objects
**resource** --> read only database containing system objects including SQL Server

#### `Advantages -->`



#### `Alternatives -->`

- There are versions of MSSQL that will run on Linux and Mac, but we mostly see MSSQL instances on targets running windows.

#### `Configuration Files -->`

- When an admin initially installs and configures MSSQL to be network accessible the SQL service will likely run as--> **NTSERVICE/MSSQLSERVER**

#### `Commands -->`



#### `Vulnerabilities -->`

-  Connecting to a MSSQL Server from client side is possible through windows authentication, and by default, **encryption** is not enforced while attempting to connect.

![[auth.png]]

>[!Note]
>Authentication being set to `Windows Authentication` means that the underlying Windows OS will process the login request and use either the local SAM database or the domain controller (hosting Active Directory) before allowing connectivity to the database management system. Using Active Directory can be ideal for auditing activity and controlling access in a Windows environment, but if an account is compromised, it could lead to privilege escalation and lateral movement across a Windows domain environment. Like with any OS, service, server role, or application, it can be beneficial to set it up in a VM from installation to configuration to understand all the default configurations and potential mistakes that the administrator could make.

So,

1. MSSQL clients not using encryption to connect to the MSSQL server

2. The use of self-signed certificates when encryption is being used. It is possible to spoof self-signed certificates

3. The use of [named pipes](https://docs.microsoft.com/en-us/sql/tools/configuration-manager/named-pipes-properties?view=sql-server-ver15)

4. Weak & default `sa` credentials. Admins may forget to disable this account

#### `Remedies -->`



#### `Footprinting Walkthrough -->`

- The scripted [[NMAP]] scan below provides us with helpful information. 

>	sudo nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 10.129.201.248

- We can also use [[metasploit]] to run the scanner **mssql_ping**

>	msfconsole
>	use auxiliary/scanner/mssql/mssql_ping

- If we can manage to retrieve credentials, this allows us to remotely connect using **mssqlclient.py**, upon which we can interact with the server using [T-SQL](https://infocenter.sybase.com/help/index.jsp?topic=/com.sybase.infocenter.dc36455.1500/html/dcdb2bok/CACCAAAF.htm).

>	python3 mssqlclient.py Administrator@10.129.201.248 -windows-auth

#### `Summary -->`

- The resource for [enumeration](https://alamot.github.io/mantis_writeup/#enumeration).