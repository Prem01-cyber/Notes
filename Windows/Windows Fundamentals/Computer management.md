
***
###### `Title :- Computer management`
###### `Created on :- 2023-08-06 - 23:22`
###### `Created by:- Prem J`
***

- We're continuing with Tools that are available through the System Configuration panel.

- The **Computer Management** (`compmgmt`) utility has three primary sections: System Tools, Storage, and Services and Applications.

![[Pasted image 20230806232817.png]]

#### `Task Scheduler -->`

- we can create and manage common tasks that our computer will carry out automatically at the times we specify, Similar to cronjobs in linux

![[Pasted image 20230806232949.png]]

#### `Event Viewer -->`

- allows us to view the events that have occurred on our computer. 
- These records of events can be seen as an audit trail that can be used to understand the activity of the computer system.
- Often used for diagnosing the computer status

![[Pasted image 20230806233057.png]]

- Event Viewer has three panes.
	1. The pane on the left provides a hierarchical tree listing of the event log providers. (as shown in the image above)
	2. The pane in the middle will display a general overview and summary of the events specific to a selected provider.
	3. The pane on the right is the actions pane.

- There are five types of events that can be logged

![[Pasted image 20230806233148.png]]

- The standard logs are visible under Windows Logs. 

![[Pasted image 20230806233321.png]]

#### `Shared Folders -->`

- Complete list of shares and folders shared that others can see.

![[Pasted image 20230806233448.png]]

- We can also view the list of users who are currently connected to the shares

#### `Local User and Groups -->`

- as mentioned in the [[Permissions and User Accounts#`User accounts -->`]] this can be accessed through `lusrmgr.exe`
#### `Performance -->`

- you'll see a utility called **Performance Monitor** (`perfmon`).
- `Perfmon` is used to view performance data either in real-time or from a log file. This utility is useful for troubleshooting performance issues on a computer system, whether local or remote.

![[Pasted image 20230806233820.png]]

#### `Device Manager -->`

- allows us to view and ***configure the hardware***, such as disabling any hardware attached to the computer.

![[Pasted image 20230806233851.png]]

#### `Storage -->`

- Under Storage is **Windows Server Backup** and **Disk Management**

![[Pasted image 20230806233939.png]]

- Disk Management is a system utility in Windows that enables you to perform advanced storage tasks.  Some tasks are:
	- Set up a new drive
	- Extend a partition
	- Shrink a partition
	- Assign or change a drive letter (ex. E:)
	- Format current drives

#### `Services and Applications -->`

- a service is a special type of application that runs in the background. Here you can do more than enable and disable a service, such as view the Properties for the service.

![[Pasted image 20230806234104.png]]

![[Pasted image 20230806234110.png]]

- WMI Control configures and controls the **Windows Management Instrumentation** (WMI) service.

>[!Note]
>WMI allows scripting languages (such as VBScript or Windows PowerShell) to manage Microsoft Windows personal computers and servers, both locally and remotely. Microsoft also provides a command-line interface to WMI called Windows Management Instrumentation Command-line (WMIC).

- The `WMIC` tool is deprecated in Windows 10, version `21H1`. Windows PowerShell supersedes this tool for WMI.