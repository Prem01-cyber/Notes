
***
###### `Title :- SQL Operators`
###### `Created on :- 2023-06-15 - 22:36`
###### `Created by:- Prem J`
***

- Sometimes, expressions with a single condition are not enough to satisfy the user's requirement. For that, SQL supports [Logical Operators](https://dev.mysql.com/doc/refman/8.0/en/logical-operators.html) to use multiple conditions at once. The most common logical operators are `AND`, `OR`, and `NOT`.

#### `AND Operator -->`

- The `AND` operator takes in two conditions and returns `true` or `false` based on their evaluation for reference [[Query Results#`Filtering Results - WHERE -->`]]

```shell-session
mysql> SELECT 1 = 1 AND 'test' = 'test';
mysql> SELECT 1 = 1 AND 'test' = 'abc';
```

>[!Note]
>In mysql `non-zero` = True and returns `1` to signify it's `true`. `0` is considered `false`

#### `OR Operator -->`

- The `OR` operator takes in two expressions as well, and returns `true` when at least one of them evaluates to `true`

```shell-session
mysql> SELECT 1 = 1 OR 'test' = 'abc';
mysql> SELECT 1 = 2 OR 'test' = 'abc';
```

#### `NOT Operator -->`

- The `NOT` operator simply toggles a `boolean` value 'i.e. `true` is converted to `false` and vice versa

```shell-session
mysql> SELECT NOT 1 = 1;
mysql> SELECT NOT 1 = 2;
```

#### `Symbol Operators -->`

- The symbolic versions of `AND` - `&&`, `OR` - `||`, `NOT` - `!` 

#### `Multiple Operator Precedence -->`

- SQL supports various other operations such as addition, division as well as bitwise operations. Thus, a query could have multiple expressions with multiple operations at once. The order of these operations is decided through operator precedence.
- Here is a list of common operations and their precedence, as seen in the [MariaDB Documentation](https://mariadb.com/kb/en/operator-precedence/):

	- Division (`/`), Multiplication (`*`), and Modulus (`%`)
	- Addition (`+`) and subtraction (`-`)
	- Comparison (`=`, `>`, `<`, `<=`, `>=`, `!=`, `LIKE`)
	- NOT (`!`)
	- AND (`&&`)
	- OR (`||`)

```sql
SELECT * FROM logins WHERE username != 'tom' AND id > 3 - 2;
```

- The query has four operations: `!=`, `AND`, `>`, and `-`. From the operator precedence, we know that subtraction comes first, so it will first evaluate `3 - 2` to `1`
- Next, we have two comparison operations, `>` and `!=`. Both of these are of the ***same precedence*** and will be ***evaluated together***. So, it will return all records where username is not `tom`, and all records where the `id` is greater than 1, and then apply `AND` to return all records with both of these conditions