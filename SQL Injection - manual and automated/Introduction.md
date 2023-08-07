
***
###### `Title :- Introduction`
###### `Created on :- 2023-06-13 - 22:19`
###### `Created by:- Prem J`
***

- Most web applications use ***Database*** Structure on backend to store and retrieve data related to the web application.
- The web applications use a ***Query Language*** to interact with the backend databases.

![[Pasted image 20230613232137.png]]

- When user-supplied information is used to construct the query to the database, malicious users can trick the query into being used for something other than what the original programmer intended, providing the user access to query the database using an attack known as ***SQL injection (SQLi)***.
- SQL injection refers to ***attacks against relational databases[^1]*** such as `MySQL` (whereas injections against non-relational databases[^2] , such as MongoDB, are NoSQL injection). This module will focus on `MySQL` to introduce SQL Injection concepts.

#### `SQLi - SQL Injection -->`

- Many types of injection vulnerabilities are possible within web applications, such as `HTTP injection`, `code injection`, and `command injection`
- Among which most common example however is SQL Injection. It occurs when malicious user attempts to change the final SQL query sent by the application to the Database to enabling the user to perform other ***unintended SQL queries*** directly against the database.
- There are many ways to accomplish this. To get a SQL injection to work, the attacker must first inject SQL code and then subvert the web application logic by changing the original query or executing a completely new one. 
- First, the attacker has to inject code outside the expected user input limits, so it does not get executed as simple user input. In the most basic case, this is **done by injecting a single quote (`'`) or a double quote (`"`) to escape the limits of user input** and inject data directly into the SQL query
- Once an attacker can inject, they have to look for a way to execute a different SQL query. This can be done using SQL code to make up a working query that executes both the intended and the new SQL queries. There are many ways to achieve this, like using [stacked](https://www.sqlinjection.net/stacked-queries/) queries or using [Union](https://www.mysqltutorial.org/sql-union-mysql.aspx/) queries. 
- Finally, to retrieve our new query's output, we have to interpret it or capture it on the web application's front end.

#### `Use Cases and Impact -->`

- A SQL injection can have a tremendous impact, especially if privileges on the back-end server and database are weak, sometimes it may lead RCE (Remote Code Execution)
- SQLi can lead to ***data breach***. Which can have dangerous outcomes like having admin level access .e.t.c.

#### `Preventions -->`

- SQL injections are usually caused by poorly coded web applications or poorly secured back-end server and databases privileges. ***Input sanitization***, ***validation*** and proper server configuration can reduce the chance of SQLi


[^1]: relational databases refer to the databases which manage data using tables (rows and columns)
[^2]: A non-relational database (also called a `NoSQL` database) does not use tables, rows, and columns or prime keys, relationships, or schema. Instead, a NoSQL database stores data using various storage models, depending on the type of data stored refer [[Types of Databases]]