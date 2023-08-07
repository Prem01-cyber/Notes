
***
###### `Title :- Processes`
###### `Created on :- 2023-08-07 - 22:32`
###### `Created by:- Prem J`
***

#### `System -->`

- PID for System is always 4.
- The System process (process ID 4) is the home for a special kind of thread that runs only in kernel mode a kernel-mode system thread.
- System threads have all the attributes and contexts of regular user-mode threads (such as a hardware context, priority, and so on) but are different in that they run only in kernel-mode executing code loaded in system space, whether that is in `Ntoskrnl.exe` or in any other loaded device driver.
- In addition, system threads don't have a user process address space and hence must allocate any dynamic storage from operating system memory heaps, such as a paged or nonpaged pool.

- System when viewed in Process explorer

![[Pasted image 20230807224456.png]]

- System when viewed in Process Hacker

![[Pasted image 20230807224906.png]]

- What is unusual behaviour for this process?
	- A parent process (aside from System Idle Process (0))
	- Multiple instances of System. (Should only be one instance) 
	- A different PID. (Remember that the PID will always be PID 4)
	- Not running in Session 0

#### `System > smss.exe -->`

- Session Manager Subsystem (`smss`), also known as Windows Session Manager. It is mainly responsible for ***creating new sessions***.
- It is the first user mode process started by the kernel.

- This process starts the kernel and user modes of the Windows subsystem (you can read more about the NT Architecture [here](https://en.wikipedia.org/wiki/Architecture_of_Windows_NT)).
- This subsystem includes `win32k.sys` (kernel mode), `winsrv.dll` (user mode), and `csrss.exe` (user mode).
- `smss.exe` starts `csrss.exe` (Windows subsystem) and `wininit.exe` in ***Session 0***, an isolated windows session for the operating system, and `csrss.exe` and `winlogon.exe` for ***Session 1***, which is the user session.
- The first child instance creates child instances in new sessions, done by `smss.exe` copying itself into the new session and self-terminating. You can read more about this process [here](https://en.wikipedia.org/wiki/Session_Manager_Subsystem).

Session 0 (`csrss.exe` & `wininit.exe`) 

![Smss.exe session 0.](https://assets.tryhackme.com/additional/windows-processes/smss-session0-tree.png)

![csrss.exe properties on session 0.](https://assets.tryhackme.com/additional/windows-processes/smss-session0b.png)


Session 1 (`csrss.exe` & `winlogon.exe`) 

![Smss.exe session 1.](https://assets.tryhackme.com/additional/windows-processes/smss-session1-tree.png)

![csrss.exe properties on session 1.](https://assets.tryhackme.com/additional/windows-processes/smss-session1b.png)

- Any other subsystem listed in the `Required` value of `HKLM\System\CurrentControlSet\Control\Session Manager\Subsystems` is also launched.

![[Pasted image 20230807225708.png]]

- `smss.exe` is also responsible for creating environmental variables, virtual memory paging files and starts `winlogon.exe` (Windows Logon Manger)

![[Pasted image 20230807225806.png]]

What is unusual?
	- A different parent process other than System (4)
	- The image path is different from `C:\Windows\System32`
	- More than one running process. (children self-terminate and exit after each new session)
	- The running User is not the SYSTEM user
	- Unexpected registry entries for Subsystem

#### `csrss.exe -->`

- Client Server Runtime Process (`csess.exe`) is the user mode side of the windows system.
- this is always running, stopping it would result in system failure
- This is responsible for:
	- `Win32` console windows
	- Process thread creation and deletion
	- Makes Windows API available to other processes
	- mapping drive letters
	- hands the shutdown process of windows

- For each instance, `csrsrv.dll`, `basesrv.dll`, and `winsrv.dll` are loaded (along with others).
- `csrss.exe` and `winlogon.exe` are called from `smss.exe` at startup for session 1

![[Pasted image 20230807230502.png]]

![[Pasted image 20230807230508.png]]

What is unusual?
	- An actual parent process. (`smss.exe` calls this process and self-terminates)
	- Image file path other than `C:\Windows\System32`
	- Subtle misspellings to hide rogue processes masquerading as `csrss.exe` in plain sight
	- The user is not the SYSTEM user.

#### `wininit.exe -->`

- The **Windows Initialization Process**, `wininit.exe`, is responsible for launching `services.exe` (Service Control Manager), `lsass.exe` (Local Security Authority), and `lsaiso.exe` within ***Session 0***.
- It is another critical Windows process that runs in the background, along with its child processes.

![[Pasted image 20230807231353.png]]

>[!note]
>`lsaiso.exe` is a process associated with **Credential Guard and KeyGuard**. You will only see this process if Credential Guard is enabled

![[Pasted image 20230807231442.png]]

- What is unusual?
	- An actual parent process. (`smss.exe` calls this process and self-terminates)
	- Image file path other than `C:\Windows\System32`
	- Subtle misspellings to hide rogue processes in plain sight
	- Multiple running instances
	- Not running as SYSTEM

#### `wininit.exe > services.exe -->`

- it is launched by `wininit.exe` which is in turn launched by `smss.exe` which is launched by `System`
- Its primary responsibility is to handle system services: loading services, interacting with services and starting or ending services. 
- It maintains a database that can be queried using a Windows built-in utility, `sc.exe`.

![[Pasted image 20230807231754.png]]

- information regarding the services is stored in `HKLM\System\CurrentControlSet\Services`

![[Pasted image 20230807231818.png]]

- This process also loads device drivers marked as auto-start into memory.
- When a user logs into a machine successfully, this process is responsible for setting the value of the Last Known Good control set (Last Known Good Configuration), `HKLM\System\Select\LastKnownGood`, to that of the `CurrentControlSet`.

![[Pasted image 20230807231904.png]]

- This process is a parent to many processes

![[Pasted image 20230807231934.png]]

![[Pasted image 20230807231937.png]]

![[Pasted image 20230807231942.png]]

- What is unusual?
	- A parent process other than `wininit.exe`
	- Image file path other than `C:\Windows\System32`
	- Subtle misspellings to hide rogue processes in plain sight
	- Multiple running instances
	- Not running as SYSTEM

#### `wininit.exe > services.exe > svchost.exe -->`

- as mentioned earlier `services.exe` is a parent to many processes one of them is `svhost.exe` (Service Host - host processes for windows services)
- It is responsible for hosting Windows services

![[Pasted image 20230807232258.png]]

