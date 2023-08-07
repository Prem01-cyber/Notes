
***
###### `Title :- Types of Databases`
###### `Created on :- 2023-06-14 - 07:01`
###### `Created by:- Prem J`
***

- There are mainly two types of databases `Relational Database` and `Non-Relational Database`

>[!Note]
>Only Relational Database use SQL, Non-relational Database uses variety methods of communication

#### `Relational Database -->`

- It uses a schema[^1], a template, to dictate the data structure stored in the database.
- Tables in a relational database are associated with keys that provide a quick database summary or access to the specific row or column when specific data needs to be reviewed. These tables, also called entities, are all related to each other.
- However, when processing an integrated database, ***a concept is required to link one table to another using its key***, called a `relational database management system` (`RDBMS`).
- it becomes rapid and easy to retrieve all data about a particular element from all databases. This makes relational databases very fast and reliable for big datasets with clear structure and design and efficient data management. 
- Many types of databases now implement the RDBMS concept, such as Microsoft Access, ***MySQL***, SQL Server, Oracle, PostgreSQL, and many others. 

![[Pasted image 20230614072341.png]]
- For example, we can have a `users` table in a relational database containing columns like `id`, `username`, `first_name`, `last_name`, and others. The `id` can be used as the table key. Another table, `posts`, may contain posts made by all users, with columns like `id`, `user_id`, `date`, `content`, and so on
- We can link the `id` from the `users` table to the `user_id` in the `posts` table to retrieve the user details for each post without storing all user details with each post. A table can have more than one key, as another column can be used as a key to link with another table. So, for example, the `id` column can be used as a key to link the `posts`table to another table containing comments, each of which belongs to a particular post, and so on.

#### `Non-relational Database -->`

- A non-relational database (also called a `NoSQL` database) does not use tables, rows, and columns or prime keys, relationships, or schemas. Instead, a NoSQL database stores data using various storage models, depending on the type of data stored.+

>[!Note]
>Due to the lack of a defined structure for the database, NoSQL databases are very scalable and flexible. Therefore, when dealing with datasets that are not very well defined and structured, a NoSQL database would be the best choice for storing such data

- There are 4 common storage models of NoSQL databases:
	1. Key-Value --> JSON or XML
	2. Document-Based
	3. Wide-Column
	4. Graph

![[Pasted image 20230614074044.png]]
- The json representation would be like 
```json
{
  "100001": {
    "date": "01-01-2021",
    "content": "Welcome to this web application."
  },
  "100002": {
    "date": "02-01-2021",
    "content": "This is the first post on this web app."
  },
  "100003": {
    "date": "02-01-2021",
    "content": "Reminder: Tomorrow is the ..."
  }
}
```


- It looks similar to a dictionary item in languages like `Python` or `PHP` (i.e. `{'key':'value'}`), where the `key` is usually a string, and the `value` can be a string, dictionary, or any class object.
- Most common NoSQL database is `MongoDB`.

[^1]: The relationship between tables within a database is called a Schema.