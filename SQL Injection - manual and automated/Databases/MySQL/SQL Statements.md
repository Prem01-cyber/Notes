
***
###### `Title :- SQL Statements`
###### `Created on :- 2023-06-15 - 07:22`
###### `Created by:- Prem J`
***

- To perform any of these below operations we need to select the database first using the command `use`

```shell-session
use employees;
```

#### `INSERT Statement -->`

```sql
INSERT INTO table_name VALUES (column1_value, column2_value, column3_value, ...);
```

- The [INSERT](https://dev.mysql.com/doc/refman/8.0/en/insert.html) statement is used to add new records to a given table. `C` in `CRUD`
- we need to pass all the required fields in the table using the command. If the not required fields are left empty, they will be filled with default values which are mentioned while creating the table as mentioned in [[SQL Injection Fundamentals/Databases/MySQL/Introduction|Introduction]]. Or else it may lead to an error.

```shell-session
mysql> INSERT INTO logins(username, password) VALUES ('john', 'john123!'), ('tom', 'tom123!');

Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0
```

- We can also add multiple records at once by separating them with a name.

```sql
INSERT INTO table_name(column2, column3, ...) VALUES (column2_value, column3_value, ...);
```

- Inserting only specific columns in the table

#### `SELECT Statement -->`

- We can retrieve data with [SELECT](https://dev.mysql.com/doc/refman/8.0/en/select.html) statement. `R` in `CRUD`

```sql
SELECT * FROM table_name;
```

- The asterisk symbol (\*) acts as a wildcard and selects all the columns. The `FROM`keyword is used to denote the table to select from. It is possible to view data present in specific columns as well.

```sql
SELECT column1, column2 FROM table_name;
```

- Upon selecting we would get the result as follows

```shell-session
mysql> SELECT * FROM logins;        #All Columns

+----+---------------+------------+---------------------+
| id | username      | password   | date_of_joining     |
+----+---------------+------------+---------------------+
|  1 | admin         | p@ssw0rd   | 2020-07-02 00:00:00 |
|  2 | administrator | adm1n_p@ss | 2020-07-02 11:30:50 |
|  3 | john          | john123!   | 2020-07-02 11:47:16 |
|  4 | tom           | tom123!    | 2020-07-02 11:47:16 |
+----+---------------+------------+---------------------+
4 rows in set (0.00 sec)


mysql> SELECT username,password FROM logins;      #specific columns
    
+---------------+------------+
| username      | password   |
+---------------+------------+
| admin         | p@ssw0rd   |
| administrator | adm1n_p@ss |
| john          | john123!   |
| tom           | tom123!    |
+---------------+------------+
4 rows in set (0.00 sec)
```

- if the tables are nested we need to select as 

```shell-session
select * from employees.departments;   #employee db and department table
```

#### `DROP Statement -->`

- We can use [DROP](https://dev.mysql.com/doc/refman/8.0/en/drop-table.html) to remove tables and databases from the server. `D` in `CRUD`

```shell-session
mysql> DROP TABLE logins;

Query OK, 0 rows affected (0.01 sec)


mysql> SHOW TABLES;

Empty set (0.00 sec)
```

>[!Danger]
>The 'DROP' statement will permanently and completely delete the table with no confirmation, so it should be used with caution.

#### `ALTER Statement -->`

- We can use [ALTER](https://dev.mysql.com/doc/refman/8.0/en/alter-table.html) to change the name of any table and any of its fields or to delete or add a new column to an ***existing table***

```shell-session
mysql> ALTER TABLE logins ADD newColumn INT;      #New Column
mysql> ALTER TABLE logins RENAME COLUMN newColumn TO oldColumn;    #Rename
mysql> ALTER TABLE logins MODIFY oldColumn DATE;    #Data type change
mysql> ALTER TABLE logins DROP oldColumn;      #Drop in alter
```

#### `UPDATE Statement -->`

- While `ALTER` is used to change a table's properties, the [UPDATE](https://dev.mysql.com/doc/refman/8.0/en/update.html) statement can be used to update specific records within a table, based on certain conditions. `U` in `CRUD`

```sql
UPDATE table_name SET column1=newvalue1, column2=newvalue2, ... WHERE <condition>;
```

```shell-session
mysql> UPDATE logins SET password = 'change_password' WHERE id > 1;

Query OK, 3 rows affected (0.00 sec)
Rows matched: 3  Changed: 3  Warnings: 0


mysql> SELECT * FROM logins;

+----+---------------+-----------------+---------------------+
| id | username      | password        | date_of_joining     |
+----+---------------+-----------------+---------------------+
|  1 | admin         | p@ssw0rd        | 2020-07-02 00:00:00 |
|  2 | administrator | change_password | 2020-07-02 11:30:50 |
|  3 | john          | change_password | 2020-07-02 11:47:16 |
|  4 | tom           | change_password | 2020-07-02 11:47:16 |
+----+---------------+-----------------+---------------------+
4 rows in set (0.00 sec)
```

>[!Note]
>we have to specify the 'WHERE' clause with UPDATE, in order to specify which records get updated. The 'WHERE' clause will be discussed next.






