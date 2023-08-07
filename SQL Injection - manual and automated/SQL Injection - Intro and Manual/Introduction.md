
***
###### `Title :- Introduction`
###### `Created on :- 2023-06-15 - 23:21`
###### `Created by:- Prem J`
***

#### `Use of SQL In Web Applications -->`

- First, let us see how web applications use databases MySQL, in this case, to store and retrieve data. 
- Once a DBMS is installed and set up on the back-end server and is up and running, the web applications can start utilizing it to store and retrieve data.
- For example, within a `PHP` web application, we can connect to our database, and start using the `MySQL` database through `MySQL` syntax, right within `PHP`

```php
$conn = new mysqli("localhost", "root", "password", "users");
$query = "select * from logins";
$result = $conn->query($query);
```

- all the query output will be stored in the result variable. further usage can be our choice.

```php
while($row = $result->fetch_assoc() ){
	echo $row["name"]."<br>";
}
```

- it will print all returned results of the SQL query in new lines.
- Web applications also usually use ***user-input when retrieving data***. 
- For example, when a user uses the ***search function*** to search for other users, their search input is passed to the web application, which uses the input to search within the databases.

```php
$searchInput =  $_POST['findUser'];
$query = "select * from logins where username like '%$searchInput'";
$result = $conn->query($query);
```

>[!Danger]
>`If we use user-input within an SQL query, and if not securely coded, it may cause a variety of issues, like SQL Injection vulnerabilities.`

- User input sanitization and validation are the main sources for SQL Injection vulnerabilites

#### `What is an Injection -->`

- In the above example we accepted the input directly to SQL query without sanitization. Injection occurs when an application misinterprets user input as actual code rather than a string, changing the code flow and executing it.
- This can occur by escaping user-input bounds by injecting a special character like (`'`), and then writing code to be executed, like JavaScript code or SQL in SQL Injections. Unless the user input is sanitized, it is very likely to execute the injected code and run it.

#### `SQL Injection -->`

- Improper input sanitization could lead to SQLi, as in the previous example of search the input is taken directly without having it properly sanitized.

```sql
select * from logins where username like '%$searchInput'
```

- if we search for admin -- `%admin`. but if we write any SQL code it becomes different like.

```sql
select * from logins where username like '%$SHOW DATABASES;'
```

- This would not affect anything. However, as there is no sanitization, in this case, **we can add a single quote (`'`), which will end the user-input field, and after it, we can write actual SQL code**. 
- For example, if we search for `1'; DROP TABLE users;`, the search input would be.

```sql
select * from logins where username like '%$1'; DROP TABLE users;
```

- making it the second query, we added a quote in order to escape the bounds of the user-input. By which we can enter our own query.

>[!tip]
>In the above example, for the sake of simplicity, we added another SQL query after a semi-colon (;). Though this is actually not possible with MySQL, it is possible with MSSQL and PostgreSQL.


#### `Syntax Error -->`

- Above SQL Injection may return an error

```php
Error: near line 1: near "'": syntax error
```

```sql
select * from logins where username like '%1'; DROP TABLE users;'
```

- That's because the web application thinks the input we have entered as an actual input and adds a trailing quote to making the query incomplete. Hence we get an error.
- To have a successful injection, we must ensure that the newly modified SQL query is still valid and does not have any syntax errors after our injection. 
- In most cases, we would not have access to the source code to find the original SQL query and develop a proper SQL injection to make a valid SQL query.
- To inject SQL query successfully, One answer is by using `comments`.
- Another is to make the query syntax work by passing in multiple single quotes

#### `Types of SQL Injections -->`

![[Pasted image 20230616072058.png]]

- SQL Injections are categorized based on how and where we retrieve their output.
- If the ***output*** can be ***viewed directly in*** the front-end and we can read it directly, this is known as ***In-band SQL Injection***. and it has two types `Union Based` and `Error Based`

- With `Union Based` SQL injection, we may have to specify the exact location, 'i.e., column', which we can read, so the query will direct the output to be printed there. 
- As for `Error Based` SQL injection, it is used when we can get the `PHP` or `SQL` errors in the front-end, and so we may intentionally cause an SQL error that returns the output of our query.

- In more complicated cases, we ***may not get the output*** printed, so we may utilize SQL logic to retrieve the output character by character. This is known as `Blind` SQL injection, and it also has two types: `Boolean Based` and `Time Based`.
- With `Boolean Based` SQL injection, we can use SQL conditional statements to control whether the page returns any output at all, 'i.e., original query response,' if our conditional statement returns `true`. 
- As for `Time Based` SQL injections, we use SQL conditional statements that delay the page response if the conditional statement returns `true` using the `Sleep()` function.

- Finally, in some cases, we ***may not have direct access*** to the output whatsoever, so we may have to direct the output to a remote location, 'i.e., DNS record,' and then attempt to retrieve it from there. This is known as `Out-of-band` SQL injection.