
***
###### `Title :- Default Passwords`
###### `Created on :- 2023-06-25 - 19:37`
###### `Created by:- Prem J`
***

- Many employees in the companies use default passwords offered by the application.
- It the password or username is incorrect `Basic HTTP authentication` will result in a HTTP 401 Unauthorised message.

#### `Hydra -->`

- `Hydra` is a handy tool for Login Brute Forcing, as it covers a wide variety of attacks and services and is relatively fast compared to the others. 
- It can test any pair of credentials and verify whether they are successful or not but in huge numbers and a very quick manner.
- it can be installed using `apt install hydra` or from a its [Github Repository](https://github.com/vanhauser-thc/thc-hydra).

#### `Default Passwords -->`

- As we don't know any of the username or password, we need to brute force both fields. We can either provide different wordlists for both fields and iterate over all possible username and password combinations.
- However, the above mentioned process should be held as the last resort.
- It is common to find pairs of usernames and passwords used together like `test:test`. Hence it is always better to start with a wordlist of such credential pairs.

```shell-session
prem@htb[/htb]$ hydra -C wordlist 178.211.23.155 -s 31099 http-get /

Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

[DATA] max 16 tasks per 1 server, overall 16 tasks, 66 login tries, ~5 tries per task
[DATA] attacking http-get://178.211.23.155:31099/
[31099][http-get] host: 178.211.23.155   login: test   password: testingpw
[STATUS] attack finished for 178.211.23.155 (valid pair found)
1 of 1 target successfully completed, 1 valid password found
```

- `-C` options indicates that we are using a combine credential wordlist in order to target the IP on port (`-s`) using `http-get` method. (`/` - Target path) 

>[!tip]
>It's pretty common for administrators to overlook test or default accounts and their credentials. That is why it is always advised to start by scanning for default credentials, as they are very commonly left unchanged. It is even worth testing for the top 3-5 most common default credentials manually, as it can very often be found to be used.

