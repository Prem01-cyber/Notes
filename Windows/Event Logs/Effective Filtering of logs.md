
***
###### `Title :- Effective Filtering of logs`
###### `Created on :- 2023-08-07 - 10:01`
###### `Created by:- Prem J`
***

#### `Wevtutil.exe -->`

- `wevtutil.exe` enables you to retrieve information about event logs and publishers. You can also use this command to install and uninstall event manifest logs, to run queries, and to export, archive and clear logs.

![[Pasted image 20230807100739.png]]

- we can use `wevutil COMMAND /?` to get additional info on that specific command.

>[!Tip]
>to get the length of the output pile the command through `Measure-Object` command

- You can get more information about using this tool further but visiting the online help documentation [docs.microsoft.com](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/wevtutil).

#### `Get-WinEvent -->`