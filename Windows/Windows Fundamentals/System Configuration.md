
***
###### `Title :- System Configuration`
###### `Created on :- 2023-08-06 - 22:17`
###### `Created by:- Prem J`
***

- The **System Configuration** utility (`MSConfig`) is for advanced troubleshooting, and its main purpose is to help diagnose startup issues.

>[!tip]
>Reference the following document [here](https://docs.microsoft.com/en-us/troubleshoot/windows-client/performance/system-configuration-utility-troubleshoot-configuration-errors) for more information on the System Configuration utility.

- there are several methods to launch system configuration 

![[Pasted image 20230806231218.png]]

- We need to have local admin rights to open this utility
- The utility has five tabs across the top. Below are the names for each tab. We will briefly cover each tab in this task. 	
	1. General
	2. Boot
	3. Services
	4. Startup
	5. Tools

![[Pasted image 20230806231255.png]]

![[Pasted image 20230806231333.png]]

![[Pasted image 20230806231340.png]]

- the services tab has all the services regardless of the state of the service at that instant

![[Pasted image 20230806231414.png]]

- The System Configuration utility is **NOT** a startup management program.
- There is a list of various utilities (tools) in the Tools tab that we can run to configure the operating system further. There is a brief description of each tool to provide some insight into what the tool is for.

![[Pasted image 20230806231509.png]]

- take a good lock at the selected command section, To run a tool, we can use the command to launch the tool via the run prompt, command prompt, or by clicking the `Launch` button.
- [[Computer management]], [[System Information]], [[Command Prompt]], [[Registry Editor]], [[Resource Monitor]], are some of the tools in system configuration and mentioned in detailed.




- What is the name of the service that lists Systems Internals as the manufacturer?`PsShutdown` 