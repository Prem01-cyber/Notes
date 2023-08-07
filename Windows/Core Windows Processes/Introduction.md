
***
###### `Title :- Introduction`
###### `Created on :- 2023-08-07 - 22:16`
###### `Created by:- Prem J`
***

#### `Task Manager -->`

- GUI based utility that allows user to control what is running on the operating system.
- Each process falls under three categories Apps, Background Processes or Windows process

![[Pasted image 20230807222504.png]]

- Task Manager is a utility you should be comfortable using, whether you're troubleshooting or performing analysis on the endpoint.

![[Pasted image 20230807222633.png]]

- This view provides some core processes - Details view
- Add some additional columns to see more information about these processes. Good columns to add are `Image path name` and `Command line`.

![[Pasted image 20230807222715.png]]

- We can add many columns as we want but most of them are not necessary

#### `Drawbacks -->`

- Parent Process information option is not present in task manager.
- This is where other utilities such as `Process Hacker` and `Process Explorer` come into play.

#### `Process Hacker -->`

![[Pasted image 20230807223009.png]]

#### `Process Explorer -->`

![[Pasted image 20230807223027.png]]

- Both have the Parent child process viewer which is absent in task manager, making them much more advanced when compared to task manager.

>[!Note]
>We can also get process info from powershell using the cmdlets like `tasklist`, `Get-Process` or `ps` and `wmic`

