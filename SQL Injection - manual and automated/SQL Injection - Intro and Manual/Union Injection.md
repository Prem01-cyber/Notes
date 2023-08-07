
***
###### `Title :- Union Injection`
###### `Created on :- 2023-06-17 - 07:22`
###### `Created by:- Prem J`
***

![[Pasted image 20230617072823.png]]

- In the above application we can find potential SQL injection search parameter, upon inserting a payload we get an error indicating it is prone to SQL Injection.

![[Pasted image 20230617072935.png]]

- before we perform Union Injection we need to find the number of columns so that we wont get an error as we query results.
- There are two methods to do that.
	1. Using `ORDER BY`
	2. Using `UNION`

#### `Using ORDER BY -->`

- We have to inject a query that sorts the results by a column we specified, 'i.e., column 1, column 2, and so on', until we get an error saying the column specified does not exist.
- For example, we can start with `order by 1`, sort by the first column, and succeed, as the table must have at least one column. Then we will do `order by 2` and then `order by 3` until we reach a number that returns an error, or the page does not show any output, which means that this column number does not exist. The final successful column we successfully sorted by gives us the total number of columns.
- If we failed at `order by 4`, this means the table has three columns, which is the number of columns we were able to sort by successfully. Let us go back to our previous example and attempt the same, with the following payload:

```sql
' order by 1-- 
' order by 2-- 
' order by 3-- 
' order by 4--   --> we get an error here indicating there are only 3
```

- Upon conclusion we can say that table has 3 columns.

#### `Using Union -->`

- In case of `ORDER BY` we did trail for columns count, here we will use `UNION` to match the columns in the original query.

```sql
cn' UNION select 1,2,3-- -
```

#### `Location of Injection -->`

- While a query may return multiple columns, the web application may only display some of them. So, if we inject our query in a column that is not printed on the page, we will not get its output.
- This is why we need to determine which columns are printed to the page, to determine where to place our injection. In the previous example, while the injected query returned 1, 2, 3, and 4, we saw only 2, 3, and 4 displayed back to us on the page as the output data:

![[Pasted image 20230617075207.png]]

- It is common that not every column is displayed back to the user.
- To test that we can get actual data from the database 'rather than just numbers,' we can use the `@@version` SQL query as a test and place it in the second column instead of the number 2.

```sql
cn' UNION select 1,@@version,3,4-- -
```

![[Pasted image 20230617075335.jpg]]

- We will get the version of the database instead of unusual junk.