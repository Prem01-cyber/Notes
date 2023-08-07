
***
###### `Title :- Page Fuzzing`
###### `Created on :- 2023-06-05 - 09:43`
###### `Created by:- Prem J`
***

- In the case we [[Directory Fuzzing]] we used the format,

`ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER\_IP:PORT/FUZZ`

- In the previous section, we found that we had access to `/blog`, but the directory returned an empty page, and we cannot manually locate any links or pages. So, we will once again utilize web fuzzing to see if the directory contains any hidden pages. However, before we start, we must find out what types of pages the website uses, like `.html`, `.aspx`, `.php`, or something else.
- Extension Fuzzing helps us finding the extensions used by the target server.

`ffuf -w /opt/useful/SecLists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://SERVER_IP:PORT/blog/indexFUZZ`

- In the case of page fuzzing we will be fuzzing the pages rather than directories

`ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/blog/FUZZ.php`
