
***
###### `Title :- Elements`
###### `Created on :- 2023-08-07 - 07:38`
###### `Created by:- Prem J`
***

- Windows event logs aren't text files that can be viewed through a text editor.
- The events in these log files are stored in a proprietary binary format with a `.evt` or `.evtx` extension.
- The log files with the `.evtx` file extension typically reside in `C:\Windows\System32\winevt\Logs`

#### `Elements of Windows event log -->`

- Event logs are crucial for troubleshooting any computer incident and help understand the situation and how to remediate it.
- There are several elements that form event logs:
	- **System Logs:** Records events associated with the Operating System segments. They may include information about hardware changes, device drivers, system changes, and other activities related to the device.
	- **Security Logs:** Records events connected to logon and logoff activities on a device. The system's audit policy specifies the events. The logs are an excellent source for analysts to investigate attempted or successful unauthorized activity.
	- **Application Logs**: Records events related to applications installed on a system. The main pieces of information include application errors, events, and warnings.
	- **Directory Service Events:** Active Directory changes and activities are recorded in these logs, mainly on domain controllers.
	- **File Replication Service Events:** Records events associated with Windows Servers during the sharing of Group Policies and logon scripts to domain controllers, from where they may be accessed by the users through the client servers.
	- **DNS Event Logs:** DNS servers use these logs to record domain events and to map out
	- **Custom Logs:** Events are logged by applications that require custom data storage. This allows applications to control the log size or attach other parameters, such as ACLs, for security purposes.

- Under this categorization, event logs can be further classified into types

![[Pasted image 20230807074207.png]]

- There are three main ways of accessing these event logs within a Windows system:
	1. **Event Viewer** (GUI-based application)
	2. **Wevtutil.exe** (command-line tool)
	3. **Get-WinEvent** (PowerShell cmdlet)

#### `Event Viewer -->`

- In any Windows system, the Event Viewer, a **Microsoft Management Console (MMC)** snap-in, can be launched by simply right-clicking the Windows icon in the taskbar and selecting **Event Viewer**.
- For the savvy sysadmins that use the CLI much of their day, Event Viewer can be launched by typing `eventvwr.msc`

![[e2ceaa065e80a6763b7a861dbd4142fb.gif]]

- It has three panes which provide a complete view over the events logged
- The middle plane displays events specific to the selected log provider
- The action pane provides a lot of options, we can also open a saved log, we can create a log and we can also filter logs in action pane

>[!Note]
>we can view event logs from another computer `Event Viewer (Local) > Connect to Another Computer...`
>![[Pasted image 20230807085956.png]]
- The standard logs we had earlier defined on the left pane are visible under **Windows Logs**, [[Computer management#`Event Viewer -->`]]

#### `Application and Service logs -->`

- This provides logs of specific applications for instance `Microsoft > Windows > PowerShell > Operational` Powershell logs operations from engine, providers and cmdlets to the windows event logs

![[Pasted image 20230807085620.png]]

