
***
###### `Title :- Directory Fuzzing`
###### `Created on :- 2023-06-05 - 07:40`
###### `Created by:- Prem J`
***

#### `Installation -->`

- `apt install ffuf -y` or [Github Repo of ffuf](https://github.com/ffuf/ffuf) 

#### `Directory fuzzing -->`

- `ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER\_IP:PORT/FUZZ`

-w --> Wordlist specification
-u --> url to be fuzzed
FUZZ --> the placeholder for the words in the wordlist

- upon entering the command the ffuf starts placing words in `http://SERVER\_IP:PORT/FUZZ`in place of the keyword FUZZ and checking if the url exists. The speed of fuzzing may vary based on the Internet speed and ping.

>[!Danger]
>We can even make it go faster if we are in a hurry by increasing the number of threads to 200, for example, with `-t 200`, but this is not recommended, especially when used on a remote site, as it may disrupt it, and cause a `Denial of Service`, or bring down your internet connection in severe cases. We do get a couple of hits, and we can visit one of them to verify that it exists:

- Empty page may indicate that the specific url may not a dedicated page, but also shows that we do not have access to it, as we do not get a HTTP 404 not found or HTTP 403 Access Denied.

![[web_fnb_blog.jpg]]

