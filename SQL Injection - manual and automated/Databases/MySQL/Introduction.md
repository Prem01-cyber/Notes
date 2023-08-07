
***
###### `Title :- Introduction`
###### `Created on :- 2023-06-14 - 07:44`
###### `Created by:- Prem J`
***

#### `Structured Query Language (SQL) -->`

- Every query language needs to follow the [ISO standard](https://en.wikipedia.org/wiki/ISO/IEC_9075) for Structured Query Language.
- Every developer uses this query language to be able to perform ***CRUD*** operations on the data in database.

#### `Command Line -->`

- The `mysql` utility is used to authenticate to and interact with a MySQL/MariaDB database.

`mysql -u root -p` --> u for username and p flag should be empty cuz if we pass anything it will stored in the bash_history as clear text
`mysql -u root -p<password>` --> This should be avoided

>[!tip]
>There shouldn't be any spaces between '-p' and the password.

- if we specify a host we would log in as that particular host else we would default to `localhost`
- The default MySQL/MariaDB port is (***3306***), but it can be configured to another port. It is specified using an uppercase `-P`, unlike the lowercase `-p` used for passwords.

`mysql -u root -h docker.hackthebox.eu -P 3306 -p` -> -h defines the host
`mysql -u root -h 172.62.18.68 -P 32329 -p` -> example from HTB

#### `Creating Database -->`

- Once we log in to the database using the `mysql` utility, we can start using SQL queries to interact with the DBMS

```shell-session
mysql> CREATE DATABASE users;    #Command to create a databse

Query OK, 1 row affected (0.02 sec)
```

```shell-session
mysql> SHOW DATABASES;           #Command to show created databases

+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| users              |
+--------------------+

mysql> USE users;               #We can switch to the users database 

Database changed
```

>[!Note]
>SQL statements ***aren't case sensitive***, which means 'USE users;' and 'use users;' refer to the same command. However, the database name is case sensitive, so we cannot do 'USE USERS;' instead of 'USE users;'. So, it is a good practice to specify statements in uppercase to avoid confusion.

#### `Tables -->`

- DBMS stores data in the form of tables (Rows and columns). Each block in a table is called as a cell.
- Every table is created with fixed set of columns, where each column is of a particular data type. A complete list of data types in MySQL can be found [here](https://dev.mysql.com/doc/refman/8.0/en/data-types.html).
- For example, let us create a table named `logins` to store user data, using the [CREATE TABLE](https://dev.mysql.com/doc/refman/8.0/en/creating-tables.html) SQL query:

```sql
CREATE TABLE logins (
    id INT,                    #Each column of particular data type
    username VARCHAR(100),
    password VARCHAR(100),
    date_of_joining DATETIME
    );                         #Fixed number of columns 
```

```shell-session
mysql> SHOW TABLES;            #A list of tables in current database

+-----------------+
| Tables_in_users |
+-----------------+
| logins          |
+-----------------+
1 row in set (0.00 sec)
```

```shell-session
mysql> DESCRIBE logins;       #list the table structure with its fields

+-----------------+--------------+
| Field           | Type         |
+-----------------+--------------+
| id              | int          |
| username        | varchar(100) |
| password        | varchar(100) |
| date_of_joining | date         |
+-----------------+--------------+
4 rows in set (0.00 sec)
```

#### `Table Properties -->`

- Within the `CREATE TABLE` query, there are many [properties](https://dev.mysql.com/doc/refman/8.0/en/create-table.html) that can be set for the table and each column. For example, we can set the `id` column to auto-increment using the `AUTO_INCREMENT` keyword, which automatically increments the id by one every time a new item is added to the table.

```sql
id INT NOT NULL AUTO_INCREMENT,
```

- `NOT NULL` constraint allows the cell to be never empty i.e. ***required field***, we can also use `UNIQUE` constraint to ensure that nothing is repeated.

```sql
username VARCHAR(100) UNIQUE NOT NULL,
```

- Now not two users will have same username.
- Another important keyword is the `DEFAULT` keyword, which is used to specify the default value. For example, within the `date_of_joining` column, we can set the default value to [Now()](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_now), which in MySQL returns the current date and time:

```sql
date_of_joining DATETIME DEFAULT NOW(),
```

- Finally, one of the most important properties is `PRIMARY KEY`, which we can use to uniquely identify each record in the table, referring to all data of a record within a table for relational databases, as previously discussed in the previous section. We can make the `id` column the `PRIMARY KEY` for this table.

```sql
PRIMARY KEY (id)
```

- The final query will something be like 

```sql
CREATE TABLE logins (
    id INT NOT NULL AUTO_INCREMENT,
    username VARCHAR(100) UNIQUE NOT NULL,
    password VARCHAR(100) NOT NULL,
    date_of_joining DATETIME DEFAULT NOW(),
    PRIMARY KEY (id)
    );
```

