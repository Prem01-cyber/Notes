
***
###### `Title :- Bypassing Web Application Protection`
###### `Created on :- 2023-07-14 - 07:10`
###### `Created by:- Prem J`
***

- If there are any web securities implemented in the target side, we have to be expecting errors while using the automated tools.
- But many mechanisms are implemented into SQLMap, which can help us successfully bypass such protections.

#### `Anti CSRF Token Bypass -->`

- One of the first lines of defence against the usage of automation tools is the incorporation of anti-CSRF (i.e., Cross-Site Request Forgery) tokens into all HTTP requests, especially those generated as a result of web-form filling.
- User gets a valid token if he visits  the page.
- While the original idea was the prevention of scenarios with malicious links, where just opening these links would have undesired consequences for unaware logged-in users.
- Hence this security feature has hardened the applications against the automation.

#### `How to Bypass CSRF -->`

- By using the `--csrf-token` option, SQLMap can help us Bypass the csrf token parameter.

>[!Note]
>Additionally, even in a case where the user does not explicitly specify the token's name via `--csrf-token`, if one of the provided parameters contains any of the common infixes (i.e. `csrf`, `xsrf`, `token`), the user will be prompted whether to update it in further requests.

```shell-session
htb[/htb]$ sqlmap -u "http://www.example.com/" --data="id=1&csrf-token=WfF1szMUHhiokx9AHFply5L2xAOfjRkE" --csrf-token="csrf-token"

        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.4.9}
|_ -| . [']     | .'| . |
|___|_  [)]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[*] starting @ 22:18:01 /2020-09-18/

POST parameter 'csrf-token' appears to hold anti-CSRF token. Do you want sqlmap to automatically update it in further requests? [y/N] y
```

#### `Unique Value Bypass -->`

- In some cases, the web application may only require unique values to be provided inside predefined parameters, this mechanism is similar to anti CSRF technique.
- In here we just need to ensure every request must have a unique value predefined for the a predefined parameter.
- We can use the `--randomize` option to evade this in SQLMap.

```shell-session
htb[/htb]$ sqlmap -u "http://www.example.com/?id=1&rp=29125" --randomize=rp --batch -v 5 | grep URI

URI: http://www.example.com:80/?id=1&rp=99954
URI: http://www.example.com:80/?id=1&rp=87216
URI: http://www.example.com:80/?id=9030&rp=36456
URI: http://www.example.com:80/?id=1.%2C%29%29%27.%28%28%2C%22&rp=16689
URI: http://www.example.com:80/?id=1%27xaFUVK%3C%27%22%3EHKtQrg&rp=40049
URI: http://www.example.com:80/?id=1%29%20AND%209368%3D6381%20AND%20%287422%3D7422&rp=95185
```

#### `Calculated Parameter Bypass -->`

- web application expects a proper parameter value to be calculated based on some other parameter value(s).
- Most often, one parameter value has to contain the message digest (e.g. `h=MD5(id)`) of another one.
- To bypass this, the option `--eval` should be used, where a valid Python code is being evaluated just before the request is being sent to the target.

```shell-session
htb[/htb]$ sqlmap -u "http://www.example.com/?id=1&h=c4ca4238a0b923820dcc509a6f75849b" --eval="import hashlib; h=hashlib.md5(id).hexdigest()" --batch -v 5 | grep URI

URI: http://www.example.com:80/?id=1&h=c4ca4238a0b923820dcc509a6f75849b
URI: http://www.example.com:80/?id=1&h=c4ca4238a0b923820dcc509a6f75849b
URI: http://www.example.com:80/?id=9061&h=4d7e0d72898ae7ea3593eb5ebf20c744
URI: http://www.example.com:80/?id=1%2C.%2C%27%22.%2C%28.%29&h=620460a56536e2d32fb2f4842ad5a08d
URI: http://www.example.com:80/?id=1%27MyipGP%3C%27%22%3EibjjSu&h=db7c815825b14d67aaa32da09b8b2d42
URI: http://www.example.com:80/?id=1%29%20AND%209978%socks4://177.39.187.70:33283ssocks4://177.39.187.70:332833D1232%20AND%20%284955%3D4955&h=02312acd4ebe69e2528382dfff7fc5cc
```

#### `IP Address Concealing -->`

- Concealing refers to making the IP address hidden from viewing or making our IP blacklisted.
- If by any chance above scenarios occur then we can use a proxy server or anonymity network TOR to connect to the target.
- A proxy can be set with the option `--proxy` (e.g. `--proxy="socks4://177.39.187.70:33283"`), where we should add a working proxy.
- Alternatively if we have a list of proxies we can can mention it through a file using the option `--proxy-file`.
- Alternatively we can use TOR. which would be using `SOCKS4` service on port `9050` or `9150`.

#### `WAF (Web Application Firewall) Bypass -->`

- Whenever we run SQLMap, As part of the initial tests, SQLMap sends a predefined malicious looking payload using a non-existent parameter name (e.g. `?pfov=...`) to test for the ***existence of a WAF*** (Web Application Firewall)
- There will be a substantial change in response, for example one of the most popular WAF solution implemented is `406 - Not Acceptable` Response.
- In case of a positive detection, to identify the actual protection mechanism, SQLMap uses a third-party library [identYwaf](https://github.com/stamparm/identYwaf), containing the signatures of 80 different WAF solutions. 
- If we wanted to skip this heuristical test altogether (i.e., to produce less noise), we can use switch `--skip-waf`.

#### `User agent Blacklisting Bypass -->`

- In case of immediate problems (e.g., HTTP error code 5XX from the start) while running SQLMap, one of the first things we should think of is the ***potential blacklisting*** of the default user-agent used by SQLMap (e.g. `User-agent: sqlmap/1.4.9 (http://sqlmap.org)`).
- This is trivial to bypass with the switch `--random-agent`, which changes the default user-agent with a randomly chosen value from a large pool of values used by browsers.

#### `Tamper Scripts -->`

- This is one of the most popular mechanisms implemented in SQLMap for bypassing WAF/IPS solutions.
- Tamper scripts are a special kind of (Python) scripts written for modifying requests just before being sent to the target, in most cases to bypass some protection.
- one of the most popular tamper scripts [between](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/between.py) is replacing all occurrences of greater than operator (`>`) with `NOT BETWEEN 0 AND #`, and the equals operator (`=`) with `BETWEEN # AND #`. This way, many primitive protection mechanisms (focused mostly on preventing XSS attacks) are easily bypassed, at least for SQLi purposes.
- Tamper scripts can be chained, one after another, within the `--tamper` option (e.g. `--tamper=between,randomcase`), where they are run based on their predefined priority
- A  priority is predefined to prevent any unwanted behavior, as some scripts modify payloads by modifying their SQL syntax (e.g. [ifnull2ifisnull](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/ifnull2ifisnull.py)). In contrast, some tamper scripts do not care about the inner content (e.g. [appendnullbyte](https://github.com/sqlmapproject/sqlmap/blob/master/tamper/appendnullbyte.py)).

|**Tamper-Script**|**Description**|
|---|---|
|`0eunion`|Replaces instances of UNION with e0UNION|
|`base64encode`|Base64-encodes all characters in a given payload|
|`between`|Replaces greater than operator (`>`) with `NOT BETWEEN 0 AND #` and equals operator (`=`) with `BETWEEN # AND #`|
|`commalesslimit`|Replaces (MySQL) instances like `LIMIT M, N` with `LIMIT N OFFSET M` counterpart|
|`equaltolike`|Replaces all occurrences of operator equal (`=`) with `LIKE` counterpart|
|`halfversionedmorekeywords`|Adds (MySQL) versioned comment before each keyword|
|`modsecurityversioned`|Embraces complete query with (MySQL) versioned comment|
|`modsecurityzeroversioned`|Embraces complete query with (MySQL) zero-versioned comment|
|`percentage`|Adds a percentage sign (`%`) in front of each character (e.g. SELECT -> %S%E%L%E%C%T)|
|`plus2concat`|Replaces plus operator (`+`) with (MsSQL) function CONCAT() counterpart|
|`randomcase`|Replaces each keyword character with random case value (e.g. SELECT -> SEleCt)|
|`space2comment`|Replaces space character ( ) with comments `/|
|`space2dash`|Replaces space character ( ) with a dash comment (`--`) followed by a random string and a new line (`\n`)|
|`space2hash`|Replaces (MySQL) instances of space character ( ) with a pound character (`#`) followed by a random string and a new line (`\n`)|
|`space2mssqlblank`|Replaces (MsSQL) instances of space character ( ) with a random blank character from a valid set of alternate characters|
|`space2plus`|Replaces space character ( ) with plus (`+`)|
|`space2randomblank`|Replaces space character ( ) with a random blank character from a valid set of alternate characters|
|`symboliclogical`|Replaces AND and OR logical operators with their symbolic counterparts (`&&` and `\|`)|
|`versionedkeywords`|Encloses each non-function keyword with (MySQL) versioned comment|
|`versionedmorekeywords`|Encloses each keyword with (MySQL) versioned comment|

>[!Note]
>To get a whole list of implemented tamper scripts, along with the description as above, switch `--list-tampers` can be used. We can also develop custom Tamper scripts for any custom type of attack, like a second-order SQLi.

#### `Miscellaneous Bypasses -->`

- `--chunked`, which splits the POST request's body into so-called "chunks." Blacklisted SQL keywords are split between chunks in a way that the request containing them can pass unnoticed.
- `HTTP parameter pollution` (`HPP`), where payloads are split in a similar way as in case of `--chunked` between different same parameter named values (e.g. `?id=1&id=UNION&id=SELECT&id=username,password&id=FROM&id=users...`), which are concatenated by the target platform if supporting it (e.g. `ASP`).