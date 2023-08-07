
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

- `Get-WinEvent` gets events from event logs and event tracing log files on local and remote computers.
- Additionally, you can combine numerous events from multiple sources into a single command and filter using XPath queries, structured XML queries, and hash table queries.

>[!Note]
>The **Get-WinEvent** cmdlet replaces the **Get-EventLog** cmdlet.

- we can obtain all event logs locally, and the list starts with classic logs first, followed by new windows event logs. it's possible to have record count 0 or null.

![[Pasted image 20230807171259.png]]

- `Get-WinEvent -ListProvider *` will result in log providers and their associated logs. 

![[Pasted image 20230807171528.png]]

- `Get-WinEvent -LogName Application | Where-Object { $_.ProviderName -Match 'WLMS' }` -> Where-Object cmdlet allows log filtering 

![[Pasted image 20230807172749.png]]

- When working with large event logs, per Microsoft, it's inefficient to send objects down the pipeline to a `Where-Object` command
- The use of the Get-WinEvent cmdlet's `FilterHashtable` parameter is recommended to filter event logs. We can achieve the same results as above by running the following command:

![[Pasted image 20230807172859.png]]

- Guidelines for defining a hash table are:  
	- Begin the hash table with an @ sign.
	- Enclose the hash table in braces {}
	- Enter one or more key-value pairs for the content of the hash table.
	- Use an equal sign (=) to separate each key from its value.

>[!Note]
>You don't need to use a semicolon if you separate each key/value with a new line, as in the screenshot above for the -FilterHashtable for `ProviderName='WLMS'`

- Below is a table that displays the accepted key/value pairs for the Get-WinEvent FilterHashtable parameter.

![[Pasted image 20230807173029.png]]

- When building a query with a hash table, Microsoft recommends making the hash table one key-value pair at a time. Event Viewer can provide quick information on what you need to build your hash table. For instance,

![[Pasted image 20230807173101.png]]

![[Pasted image 20230807173119.png]]

![[Pasted image 20230807173158.png]]

- [documentation](https://learn.microsoft.com/en-us/powershell/scripting/samples/creating-get-winevent-queries-with-filterhashtable?view=powershell-7.3&viewFallbackFrom=powershell-7.1) a recommended place to verify the commands

#### `Filtering using XPath Queries -->`

- XPath or XML Path language. 
- The Windows Event Log supports a subset of [XPath 1.0](https://www.w3.org/TR/1999/REC-xpath-19991116/)

![[Pasted image 20230807175249.png]]

- Based on [docs.microsoft.com](https://docs.microsoft.com/en-us/windows/win32/wes/consuming-events#xpath-10-limitations), an XPath event query starts with '`*`' or '**Event**'. The above code block confirms this. But how do we construct the rest of the query? Luckily the Event Viewer can help us with that.
- Both `wevtutil` and `Get-WinEvent` support XPath queries as event filters.

![[Pasted image 20230807175511.png]]

- The first tag here is `Event`, which is a valid one for a XPath option. the command so far will look something like `Get-WinEvent -LogName Application -FilterXPath '*'`

![[Pasted image 20230807180140.png]]

- When we get to system the command would be something like `Get-WinEvent -LogName Application -FilterXPath '/System/'`

![[Pasted image 20230807180247.png]]

- the event id is 100, now command would look something like `Get-WinEvent -Logname Application -FilterXPath '/System/EventID=100'`

![[Pasted image 20230807180445.png]]

![[Pasted image 20230807180512.png]]

![[Pasted image 20230807180521.png]]

- we can also combine two queries using the option `and`, for instance `Get-WinEvent -LogName Application -FilterXPath '*/System/EventID=101 and */System/Provider[@Name="WLMS"]'`
- we can also create XPath queries for elements within `EventData`

![[Pasted image 20230807180655.png]]

- `Get-WinEvent -LogName Security -FilterXPath '/EventData/Data[Name="TargetUserName"]="System"' -MaxEvents=1`

![[Pasted image 20230807180853.png]]

>[!Tip]
>The `-MaxEvents` parameter was used, and it was set to 1. This will return just 1 event.

- Examples

![[Pasted image 20230807182948.png]]

