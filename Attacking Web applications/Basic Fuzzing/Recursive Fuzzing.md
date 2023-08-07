
***
###### `Title :- Recursive Fuzzing`
###### `Created on :- 2023-06-05 - 10:32`
###### `Created by:- Prem J`
***

- Recursive does everything for i.e it find directories and files under it, It becomes handy when there are dozens of directories for fuzzing.

#### `Recursive flags -->`

- When we scan recursively, it automatically starts another scan under any newly identified directories that may have on their pages until it has fuzzed the main website and all of its subdirectories.
- If the websites have big tree of sub directories then the scan might also be delayed. This can be monitored using the **depth** flag. Indicating this ffuf won't go any further than specified depth.
- Recursion can be enabled with the flag **-recursion**, we can specify the depth using **-recursion-depth**.
- We can also specify the extension using **-e**.

`ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER\_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v`
