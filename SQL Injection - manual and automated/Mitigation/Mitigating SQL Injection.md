
***
###### `Title :- Mitigating SQL Injection`
###### `Created on :- 2023-06-18 - 09:43`
###### `Created by:- Prem J`
***

#### `Input Sanitization -->`

```php
<SNIP>
  $username = $_POST['username'];
  $password = $_POST['password'];

  $query = "SELECT * FROM logins WHERE username='". $username. "' AND password = '" . $password . "';" ;
  echo "Executing query: " . $query . "<br /><br />";

  if (!mysqli_query($conn ,$query))
  {
          die('Error: ' . mysqli_error($conn));
  }

  $result = mysqli_query($conn, $query);
  $row = mysqli_fetch_array($result);
<SNIP>
```

- Above is the code that we have bypassed the authentication.
- As we can see, the script takes in the `username` and `password` from the POST request and passes it to the query directly. Allowing the attacker inject anything they wish by which exploiting the system.
- This can be avoided by sanitizing the user input, rendering injected queries useless.
- Libraries provide multiple functions to achieve this, one such example is the [mysqli_real_escape_string()](https://www.php.net/manual/en/mysqli.real-escape-string.php) function. This function escapes characters such as `'`and `"`, so they don't hold any special meaning.

```php
<SNIP>
$username = mysqli_real_escape_string($conn, $_POST['username']);
$password = mysqli_real_escape_string($conn, $_POST['password']);

$query = "SELECT * FROM logins WHERE username='". $username. "' AND password = '" . $password . "';" ;
echo "Executing query: " . $query . "<br /><br />";
<SNIP>
```

![[Pasted image 20230618192818.png]]

- Upon adding the function the snippet just avoids the `'` and `"`. As expected, the injection no longer works due to escaping the single quotes. A similar example is the [pg_escape_string()](https://www.php.net/manual/en/function.pg-escape-string.php) which used to escape PostgreSQL queries.

#### `Input Validation -->`

- User input can be validated based on the data used to query to ensure that it matches the expected input. i.e email --> .....@gmail.com.
- This can be  achieved using regular expressions.  

```php
$pattern = "/^[A-Za-z\s]+$/";
$code = $_GET["port_code"];
```

- The pattern used is `[A-Za-z\s]+`, which will only match strings containing letters and spaces. Any other character will result in the termination of the script

#### `User Privileges -->`

- DBMS software allows the creation of users with fine-grained permissions. We should ensure that the user querying the database only has minimum permissions.
- **Superusers** and **users with administrative privileges** should never be used with web applications. These accounts have access to functions and features, which could lead to server compromise.

```shell-session
MariaDB [(none)]> CREATE USER 'reader'@'localhost';

Query OK, 0 rows affected (0.002 sec)


MariaDB [(none)]> GRANT SELECT ON ilfreight.ports TO 'reader'@'localhost' IDENTIFIED BY 'p@ssw0Rd!!';

Query OK, 0 rows affected (0.000 sec)
```

- User is created and granted with only the select permissions.

```shell-session
premjampuram@htb[/htb]$ mysql -u reader -p

MariaDB [(none)]> use ilfreight;
MariaDB [ilfreight]> SHOW TABLES;

+---------------------+
| Tables_in_ilfreight |
+---------------------+
| ports               |
+---------------------+
1 row in set (0.000 sec)


MariaDB [ilfreight]> SELECT SCHEMA_NAME FROM INFORMATION_SCHEMA.SCHEMATA;

+--------------------+
| SCHEMA_NAME        |
+--------------------+
| information_schema |
| ilfreight          |
+--------------------+
2 rows in set (0.000 sec)


MariaDB [ilfreight]> SELECT * FROM ilfreight.credentials;
ERROR 1142 (42000): SELECT command denied to user 'reader'@'localhost' for table 'credentials'
```

#### `Web Application Firewall -->`

- Web Application Firewalls (WAF) are used to detect malicious input and reject any HTTP requests containing them.
- They are available open source (ModSecurity) or premium (Cloudflare).
- most of them have default instructions and any request containing `INFORMATION_SCHEMA` is rejected.

#### `Parametrized Queries -->`

- Parameterized queries contain placeholders for the input data, which is then escaped and passed on by the drivers. Instead of directly passing the data into the SQL query, we use placeholders and then fill them with PHP functions.

```php
<SNIP>
  $username = $_POST['username'];
  $password = $_POST['password'];

  $query = "SELECT * FROM logins WHERE username=? AND password = ?" ;
  $stmt = mysqli_prepare($conn, $query);
  mysqli_stmt_bind_param($stmt, 'ss', $username, $password);
  mysqli_stmt_execute($stmt);
  $result = mysqli_stmt_get_result($stmt);

  $row = mysqli_fetch_array($result);
  mysqli_stmt_close($stmt);
<SNIP>
```

- The query is modified to contain two placeholders, marked with `?` where the username and password will be placed. We then bind the username and password to the query using the [mysqli_stmt_bind_param()](https://www.php.net/manual/en/mysqli-stmt.bind-param.php) function. This will safely escape any quotes and place the values in the query