
***
###### `Title :- MySQL`
###### `Created on :- 2023-05-24 - 21:16`
###### `Created by:- Prem J`
***
#### `Brief -->`

- It is a Open-source Relational Database Management System controlled by SQL database language. The data is stored in a way to take up little space so that the system can quickly process large amounts of data.
- MySQL uses the **TCP Port 3306**. Here is a small reference to [[Hashes]]
#### `Used For -->`

- MySQL is suitable for **dynamic websites**[^2].
- It is often combined with [LAMP](https://en.wikipedia.org/wiki/LAMP_(software_bundle)) (Linux, Apache, MySQL, PHP), or when using Nginx, as [LEMP](https://lemp.io/) 

>[!info]
>`MariaDB`, which is often connected with MySQL, is a fork of the original MySQL code. This is because the chief developer of MySQL left the company `MySQL AB` after it was acquired by `Oracle` and developed another open-source SQL database management system based on the source code of MySQL and called it MariaDB.

#### `How does it work -->`

- It works according **client - server principle**[^1].
- The data is stored in **tables** with different **rows and columns**.
- These databases are stored in a single file .**sq**l.
- MySQL database translates the commands internally into executable code and performs requested actions.

#### `Advantages -->`

- The data is stored in a way that it occupies little space.
- CRUD operations are performed efficiently.

#### `Alternatives -->`



#### `Configuration Files -->`

- Default Configuration --> **/etc/mysql/mysql.conf.d/mysqld.cnf**.
- There are some setting that are need to be kept in mind so that we can keep our database secured

user --> sets which user the MySQL service will run as
password -->
admin_address --> The IP address on which to listen for TCP/IP connections on the administrative network interface

- The above are security relevant

debug --> current debug settings
sql_warnings --> warnings format and what the user can see
secure_file_priv --> limit data import and export operations

#### `Commands -->`

- These are the MySQL [commands](https://gist.github.com/ashish2199/8ad29d80f3195ce3166bee55b2624653).

| **Command** | **Description** |
| --- | --- |
| `mysql -u <user> -p<password> -h <IP address>` | Connect to the MySQL server. There should **not** be a space between the '-p' flag, and the password. |
| `show databases;` | Show all databases. |
| `use <database>;` | Select one of the existing databases. |
| `show tables;` | Show all available tables in the selected database. |
| `show columns from <table>;` | Show all columns in the selected database. |
| `select * from <table>;` | Show everything in the desired table. |
| `select * from <table> where <column> = "<string>";` | Search for needed `string` in the desired table. |

>[!Note]
>You will want to have MySQL installed on your system to connect to the remote MySQL server. In case this isn't already installed, you can install it using `sudo apt install default-mysql-client`. Don't worry- this won't install the server package on your system- just the client.

- manually or with a set of non-Metasploit tools such as nmap's mysql-enum script: [https://nmap.org/nsedoc/scripts/mysql-enum.html](https://nmap.org/nsedoc/scripts/mysql-enum.html) or [https://www.exploit-db.com/exploits/23081](https://www.exploit-db.com/exploits/23081).
#### `Vulnerabilities -->`

1. Sensitive data such as passwords can be stored in **plain-text** form by MySQL.
2. Allowing User to enter any possible commands without a filter it may lead to various kind of attacks like **SQL Injection**[^2].
3. Keep a note of [Security Issues](https://dev.mysql.com/doc/refman/8.0/en/general-security-issues.html).

#### `Remedies -->`

1. However they are generally encrypted beforehand by PHP scripts using [One-Way Encryption](https://en.citizendium.org/wiki/One-way_encryption).
2. Just add checks to user input.

#### `Footprinting Walkthrough -->`

- The tester starts the footprinting process by scanning the target with [[Nmap]].

>	sudo nmap 10.129.14.128 -sV -sC -p3306 --script mysql*

>[!Note]
>As with all our scans, we must be careful with the results and manually confirm the information obtained because some of the information might turn out to be a false-positive. This scan above is an excellent example of this, as we know for a fact that the target MySQL server does not use an empty password for the user `root`, but a fixed password. We can test this by **interacting with the MySQL server**.

- To confirm the results obtained through the nmap report we interact with server.

>	mysql -u root -h 10.129.14.132
>	mysql -u root -pP4SSw0rd -h 10.129.14.128

- In the first case we are using the root password with nothing which is what the report of scan stated, but we get an error stating the password is not empty. where as in the second case we just used a password that we guessed so we get logged in if the guessed password is true. 
- Once we are connected to the server we may or may not find several tables by we will find two important files:

**system schema(sys)** --> tables information and metadata necessary for management
**information schema(information_schema)** --> database that contains metadata

#### `Summary -->`


[^1]: Client will be us who performs CRUD operations on the server.
[^2]: Refer to the notebook starting pages