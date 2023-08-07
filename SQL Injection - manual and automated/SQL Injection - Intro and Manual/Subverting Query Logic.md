
***
###### `Title :- Subverting Query Logic`
###### `Created on :- 2023-06-16 - 20:21`
###### `Created by:- Prem J`
***

- Subverting means modifying the original, we will be using modifying the original query to perform the SQL Injection

#### `Authentication Bypass -->`

![[Pasted image 20230616204106.png]]

- Above is a basic authentication page, upon logging in with `admin` and `password` the SQL query would be something like

```sql
SELECT * FROM logins WHERE username='admin' AND password = 'p@ssw0rd';
```

- If `MySQL` returns matched records the credentials would be valid (implying the search query returned true) and the user would be logged in. If the search query returned false the login attempt would be failed.

#### `SQLi Discovery -->`

- Before we start subverting the logic we need to verify if the application is vulnerable to SQL Injection by adding some payloads and checking for errors.

|Payload|URL Encoded|
|---|---|
|`'`|`%27`|
|`"`|`%22`|
|`#`|`%23`|
|`;`|`%3B`|
|`)`|`%29`|

>[!Note]
>In some cases, we may have to use the URL encoded version of the payload. An example of this is when we put our payload directly in the URL 'i.e. HTTP GET request'.

![[Pasted image 20230616220101.png]]

- Upon adding the `'` in the query we get an error stating that the login has failed due to the quote we added.

```sql
SELECT * FROM logins WHERE username=''' AND password = 'something';
```

- upon observation the problem was the triple quotes so, One option would be to comment out the rest of the query and write the remainder of the query as part of our injection to form a working query.
- Another option is to use an even number of quotes within our injected query, such that the final query would still work, which would do Nothing I guess ???

#### `OR Injection -->`

- The target here is to make the manipulated query return true, regardless of the username and password, which is the result of the authentication bypass.
- When we introduce an `OR` in the authentication query there would be two operators `AND` and `OR`.
- Here is where Operator Precedence comes into play, `AND` would be executed first according to the [[SQL Operators#`Multiple Operator Precedence -->`]]. 
- So when `AND` is done it then comes to `OR`, ***which results true even one of the both queries is true***.

- based on the above observation we just need to add an other statement which is true always. Hence the final query would be something like

```sql
SELECT * FROM logins WHERE username='admin' or '1'='1' AND password = 'something';
```

- First `AND` gets evaluated and return false then `OR` gets evaluated and results true. Making the whole query `true`. Here we can see the authentication is bypassed.

![[Pasted image 20230616221117.png]]

>[!tip]
>The payload we used above is one of many auth bypass payloads we can use to subvert the authentication logic. You can find a comprehensive list of SQLi auth bypass payloads in [PayloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection#authentication-bypass), each of which works on a certain type of SQL queries.

- The same goes for if we don't even know the username

![[Pasted image 20230616221259.png]]
