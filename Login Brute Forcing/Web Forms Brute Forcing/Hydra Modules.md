
***
###### `Title :- Hydra Modules`
###### `Created on :- 2023-06-25 - 22:06`
###### `Created by:- Prem J`
***

- Many admin panels have also implemented features or elements such as the [b374k shell](https://github.com/b374k/b374k) that might allow us to execute OS commands directly

#### `Login.php -->`

- While performing a brute force attack on a login form in order to reduce the network traffic as possible we will use the most popular admin credentials to brute force.
- If we don't get lucky we then resort to a widespread attack method called password spraying. This attack method is based on ***reusing already found***, guessed, or decrypted passwords across multiple accounts.

#### `Brute Forcing Forms -->`

- `Hydra` provides many different types of requests we can use to brute force different services like

```shell-session
hydra -h | grep "Supported services" | tr ":" "\n" | tr " " "\n" | column -e

Supported			        ldap3[-{cram|digest}md5][s]	rsh
services			        memcached					rtsp
				            mongodb						s7-300
adam6500			        mssql						sip
asterisk			        mysql						smb
cisco				        nntp						smtp[s]
cisco-enable		        oracle-listener				smtp-enum
cvs				            oracle-sid					snmp
firebird			        pcanywhere					socks5
ftp[s]				        pcnfs						ssh
http[s]-{head|get|post}		pop3[s]						sshkey
http[s]-{get|post}-form		postgres					svn
http-proxy		        	radmin2						teamspeak
http-proxy-urlenum		    rdp				  		    telnet[s]
icq				            redis						vmauthd
imap[s]		        		rexec						vnc
irc				            rlogin						xmpp
ldap2[s]		        	rpcap
```

- In this situation there are only two types of `http` modules interesting for us:

	1. `http[s]-{head|get|post}`
	2. `http[s]-post-form`

- The 1st module serves for basic HTTP authentication, while the 2nd module is used for login forms, like `.php` or `.aspx` and others.
- To decide which module we need, we have to determine whether the web application uses `GET` or a `POST` form. We can test it by trying to log in and pay attention to the URL
- Upon credential input if we find the input in the URL then its a `GET`, if we don't find the input then it is `POST`
- Based on the URL scheme at the beginning, we can determine whether this is an `HTTP` or `HTTPS` post-form
- To find out how to use the `http-post-form` module, we can use the "`-U`" flag to list the parameters it requires and examples of usage:

```shell-session
premjampuram@htb[/htb]$ hydra http-post-form -U

<...SNIP...>
Syntax:   <url>:<form parameters>:<condition string>[:<optional>[:<optional>]
First is the page on the server to GET or POST to (URL).
Second is the POST/GET variables ...SNIP... usernames and passwords being replaced in the
 "^USER^" and "^PASS^" placeholders
The third is the string that it checks for an *invalid* login (by default)
 Invalid condition login check can be preceded by "F=", successful condition
 login check must be preceded by "S=".

<...SNIP...>

Examples:
 "/login.php:user=^USER^&pass=^PASS^:incorrect"
```

- In summary, we need to provide three parameters, separated by `:`, as follows:

	1. `URL path`, which holds the login form
	2. `POST parameters` for username/password
	3. `A failed/success login string`, which lets hydra recognize whether the login attempt was successful or not

- So, `URL path` - `/login.php`, `POST params` - `[user parameter]=^USER^&[password parameter]=^PASS^`, failed/successful login attempt string - `[FAIL/SUCCESS]=[success/failed string]` and joining them with `:`

`/login.php:[user parameter]=^USER^&[password parameter]=^PASS^:[FAIL/SUCCESS]=[success/failed string]`

>[!Note]
>We may not know what the page looks like after login so we don't know the success the string hence we cannot specify a success string to look for.

#### `Fail/Success String -->`

- To make it possible to distinguish between successfully submitted credentials and failed attempts, we have to specify a unique string from the source code of the page we're using to login.
- Hydra will examine the HTML code of the response page it gets after each attempt, looking for the string we provided.

|**Type**|**Boolean Value**|**Flag**|
|---|---|---|
|`Fail`|FALSE|`F=html_content`|
|`Success`|TRUE|`S=html_content`|

- If we provide a `fail` string, it will keep looking until the string is **not found** in the response. Another way is if we provide a `success` string, it will keep looking until the string is **found** in the response.
- A better strategy is to pick something from the HTML source of the login page as the fail string. What we have to pick should be very _unlikely_ to be present after logging in, like the **login button** or the _password field_.

```html
<form name='login' autocomplete='off' class='form' action='' method='post'>
```

- We see it in a couple of places as title/header, and we find our button in the HTML form shown above. We do not have to provide the entire string. 
- so we will use `<form name='login'`, which should be distinct enough and will probably not exist after a successful login.
- So, our syntax for the `http-post-form` should be as follows:

```bash
"/login.php:[user parameter]=^USER^&[password parameter]=^PASS^:F=<form name='login'"
```
