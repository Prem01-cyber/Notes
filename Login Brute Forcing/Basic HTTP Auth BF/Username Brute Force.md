
***
###### `Title :- Username Brute Force`
###### `Created on :- 2023-06-25 - 20:35`
###### `Created by:- Prem J`
***

#### `Wordlist -->`

- One of the most commonly used password wordlists is `rockyou.txt`, which has over 14 million unique passwords, unless if the password is unique `rockyou.txt` will contain the password.
- we could download this wordlist from the [Hashcat GitHub Repository](https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt)

#### `Username/Password Attack -->`

- Hydra requires 3 specific flags if the credentials are in one single list to perform a brute force attack against a webserver.

	1. Credentials (Wordlist)
	2. Target Host (Server IP)
	3. Target Path

- Credentials can also be separated unlike in [[Default Passwords#`Hydra -->`]]  where we used a combine wordlist.
- we can use `-L` flag for the usernames wordlist and `-P` flag for the passwords wordlist.
- hydra stops brute forcing after it's first successful finding if we mention the flag `-f`

```shell-session
hydra -L /opt/useful/SecLists/Usernames/Names/names.txt -P /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt -u -f 178.35.49.134 -s 32901 http-get /
```

>[!tip]
>We will add the "-u" flag, so that it tries all users on each password, instead of trying all 14 million passwords on one user, before moving on to the next.

- Sometimes the results might appear fast as the username and password might be at the front, but sometimes it may take longer as the username and password might have been buried deep in the wordlist.

#### `Username Brute Force -->`

- In case if we know the username and only want to brute force the password then we make the username static, if we know the password then vice versa.

>[!Note]
>`-L` for wordlist, `-l` for static word and `-P` for wordlist, `-p` for static

```shell-session
hydra -L /opt/useful/SecLists/Usernames/Names/usernames.txt -p amormio -u -f 178.35.49.134 -s 32901 http-get /
```

- In the above example the password `-p` is static where as the username was given a wordlist as we don't know the username.