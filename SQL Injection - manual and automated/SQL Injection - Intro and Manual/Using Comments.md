
***
###### `Title :- Using Comments`
###### `Created on :- 2023-06-16 - 22:15`
###### `Created by:- Prem J`
***

#### `Comments -->`

- There are two types of comments in MySQL `-- ` and `#`, in addition to an in-line comment `/**/`

>[!Note]
>`/**/` is generally not used in SQLi, `-- ` is used

```shell-session
SELECT username FROM logins; -- Selects usernames from the logins table 

SELECT \* FROM logins WHERE username = 'admin'; # You can place anything here AND password \= 'something'
```

>[!Note]
>In SQL, using two dashes only is not enough to start a comment. So, there has to be an empty space after them, so the comment starts with (-- ), with a space at the end. This is sometimes URL encoded as (--+), as spaces in URLs are encoded as (+). To make it clear, we will add another (-) at at the end (-- -), to show the use of a space character.

>[!Note]
>if you are inputting your payload in the URL within a browser, a (#) symbol is usually considered as a tag, and will not be passed as part of the URL. In order to use (#) as a comment within a browser, we can use '%23', which is an URL encoded (#) symbol.

- The server ignores anything after the comment i.e `-- `, `#` (alternate `%23` - encoded)

#### `Auth Bypass with comments -->`

- In [[Subverting Query Logic]] we used `OR` operator to bypass the authentication, we can also use comments to bypass the authentication.

```sql
SELECT * FROM logins WHERE username='admin'-- ' AND password = 'something';
```

- Similar to the above scenario the query after the comment is ignored. And giving us access to the admin.

#### `Scenaraio 2 -->`

- What if there are two separate login mechanisms, one for admin and other for normal users.

![[Pasted image 20230616225036.png]]

>[!Note]
>Parenthesis have higher precedence than any of them, so they get solved first no matter what.

- above scenario is for normal users because admin may have and id `0`, so any user will be higher than that. Hence even if we have the right admin password we won't be able to login.
- Hence we will be able to login with user who has his id not equal to 1.

![[Pasted image 20230616224747.png]]

- in here we need to neglect the `id > 1` condition and replace with a true condition and we also need to ignore the entire password query as we don't know the password.
- So the final solution is to ignore the entire query from `AND`, By adding a comment to the username field i.e `admin')-- `. Making the final query like

```sql
SELECT * FROM logins where (username='admin')
```

- Hence authentication Bypassed.