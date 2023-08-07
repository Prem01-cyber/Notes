
***
###### `Title :- Handling SQLMap Errors`
###### `Created on :- 2023-06-22 - 21:29`
###### `Created by:- Prem J`
***

- While working with SQLMap we may face many errors some of which might be understood while some might be complicated.

#### `Display errors -->`

- The first step is usually to switch the `--parse-errors`, to parse the DBMS errors (if any) and displays them as part of the program run.

```shell-session
[16:09:20] [INFO] testing if GET parameter 'id' is dynamic
[16:09:20] [INFO] GET parameter 'id' appears to be dynamic
[16:09:20] [WARNING] parsed DBMS error message: 'SQLSTATE[42000]: Syntax error or access violation: 1064 You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '))"',),)((' at line 1'"
[16:09:20] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[16:09:20] [WARNING] parsed DBMS error message: 'SQLSTATE[42000]: Syntax error or access violation: 1064 You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''YzDZJELylInm' at line 1'
```

- With parse errors option enabled SQLMap will automatically print the errors in the output.

#### `Store the traffic -->`

```shell-session
prem@htb[/htb]$ sqlmap -u "http://www.target.com/vuln.php?id=1" --batch -t /tmp/traffic.txt
```

- The `-t` options stores the traffic in the specified location.

##### `Verbose Output -->`

```shell-session
prem@htb[/htb]$ sqlmap -u "http://www.target.com/vuln.php?id=1" -v 6 --batch
```

- by mentioning the level of verbosity we can raise it's level. Making the output much more detailed. `-v 6` option will directly print all the errors and full HTTP request to the terminal so that we can follow along with everything SQLMap is doing.

#### `Using Proxy -->`

- Finally, we can utilize the `--proxy` option to redirect the whole traffic through a (MiTM) proxy (e.g., `Burp`). This will route all SQLMap traffic through `Burp`, so that we can later manually investigate all requests, repeat them, and utilize all features of `Burp` with these requests

![[Pasted image 20230622214745.png]]

