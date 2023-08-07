
***
###### `Title :- Query Results`
###### `Created on :- 2023-06-15 - 22:15`
###### `Created by:- Prem J`
***

#### `Sorting Results - ORDER BY -->`

- We can sort the results of any query using [ORDER BY](https://dev.mysql.com/doc/refman/8.0/en/order-by-optimization.html) and specifying the column to sort by.

```shell-session
mysql> SELECT * FROM logins ORDER BY password;

+----+---------------+------------+---------------------+
| id | username      | password   | date_of_joining     |
+----+---------------+------------+---------------------+
|  2 | administrator | adm1n_p@ss | 2020-07-02 11:30:50 |
|  3 | john          | john123!   | 2020-07-02 11:47:16 |
|  1 | admin         | p@ssw0rd   | 2020-07-02 00:00:00 |
|  4 | tom           | tom123!    | 2020-07-02 11:47:16 |
+----+---------------+------------+---------------------+
4 rows in set (0.00 sec)
```

- default is the ascending order, but we can also sort the results by `ASC` - Ascending or `DESC` - Decending.

`SELECT * FROM logins ORDER BY password ASC;`

- It is also possible to sort by multiple columns, to have a secondary sort for duplicate values in one column.

```shell-session
mysql> SELECT * FROM logins ORDER BY password DESC, id ASC;

+----+---------------+-----------------+---------------------+
| id | username      | password        | date_of_joining     |
+----+---------------+-----------------+---------------------+
|  1 | admin         | p@ssw0rd        | 2020-07-02 00:00:00 |
|  2 | administrator | change_password | 2020-07-02 11:30:50 |
|  3 | john          | change_password | 2020-07-02 11:47:16 |
|  4 | tom           | change_password | 2020-07-02 11:50:20 |
+----+---------------+-----------------+---------------------+
4 rows in set (0.00 sec)
```

#### `Limiting Results - LIMIT -->`

- we can  [LIMIT](https://dev.mysql.com/doc/refman/8.0/en/limit-optimization.html) the results to what we want only, using `LIMIT` and the number of records we want.

```shell-session
mysql> SELECT * FROM logins LIMIT 2;

+----+---------------+------------+---------------------+
| id | username      | password   | date_of_joining     |
+----+---------------+------------+---------------------+
|  1 | admin         | p@ssw0rd   | 2020-07-02 00:00:00 |
|  2 | administrator | adm1n_p@ss | 2020-07-02 11:30:50 |
+----+---------------+------------+---------------------+
2 rows in set (0.00 sec)
```

- If we wanted to LIMIT[^1]  results with an offset, we could specify the offset before the LIMIT count.

```shell-session
mysql> SELECT * FROM logins LIMIT 1, 2;

+----+---------------+------------+---------------------+
| id | username      | password   | date_of_joining     |
+----+---------------+------------+---------------------+
|  2 | administrator | adm1n_p@ss | 2020-07-02 11:30:50 |
|  3 | john          | john123!   | 2020-07-02 11:47:16 |
+----+---------------+------------+---------------------+
2 rows in set (0.00 sec)
```

#### `Filtering Results - WHERE  -->`

- To filter or search for specific data, we can use conditions with the `SELECT` statement using the [WHERE](https://dev.mysql.com/doc/refman/8.0/en/where-optimization.html) clause, to fine-tune the results

`SELECT * FROM table_name WHERE <condition>;`

- the condition depends on what we want to filter among the results.

>[!Note]
>String and date data types should be surrounded by single quote (') or double quotes ("), while numbers can be used directly.

`select * from employees where first_name like 'Bar%' and hire_date='1990-01-01';`

#### `LIKE Clause -->`

- Another useful SQL clause is [LIKE](https://dev.mysql.com/doc/refman/8.0/en/pattern-matching.html), enabling selecting records by matching a certain pattern

```shell-session
mysql> SELECT * FROM logins WHERE username LIKE 'admin%';

+----+---------------+------------+---------------------+
| id | username      | password   | date_of_joining     |
+----+---------------+------------+---------------------+
|  1 | admin         | p@ssw0rd   | 2020-07-02 00:00:00 |
|  4 | administrator | adm1n_p@ss | 2020-07-02 15:19:02 |
+----+---------------+------------+---------------------+
2 rows in set (0.00 sec)
```

- We can use wild cards to filter results using like some of them are `%` - one or many `-` - Only one char

[^1]:  OFFSET dictates the number of rows to skip from the beginning of the returned data before presenting the results