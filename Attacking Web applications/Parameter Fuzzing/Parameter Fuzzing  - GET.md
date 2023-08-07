
***
###### `Title :- Parameter Fuzzing  - GET`
###### `Created on :- 2023-06-06 - 21:45`
###### `Created by:- Prem J`
***

- while performing fuzzing we receive all kinds of links available so some might need authentication or verification to proceed further. We might need to login to view content inside the link.
- So, perhaps there is a key that we can pass to the page to read the `flag`. Such keys would usually be passed as a `parameter`, using either a `GET` or a `POST` HTTP request

>[!tip]
>Fuzzing parameters may expose unpublished parameters that are publicly accessible. Such parameters tend to be less tested and less secured, so it is important to test such parameters for the web vulnerabilities

#### `Get Request Fuzzing -->`

- We will use ffuf to **enumerate parameters**. We start by fuzzing for GET requests, which are passed right after url with a `?`.

`http://admin.academy.htb:PORT/admin/admin.php?param1=key.`

- we will be fuzzing the param1 and key, we need to filter the results to fetch the desired output. Below command will be a basic format for the fuzzing

`ffuf -w /opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php?FUZZ=key -fs xxx`

