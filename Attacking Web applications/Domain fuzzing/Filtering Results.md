
***
###### `Title :- Filtering Results`
###### `Created on :- 2023-06-06 - 21:20`
###### `Created by:- Prem J`
***

- The results are automatically filtered by default by their HTTP code, which filters out code `404 NOT FOUND`, and keeps the rest. However, as we saw in our previous run of `ffuf`, we can get many responses with code `200`. 

#### `Filtering Results -->`

- ffuf provides an option to filter the results based on our requirement i.e
- `-m ` indicates matching `-f ` indicates filter

1. HTTP Codes `-mc`  `-fc`--> 200,204,301,302,307,401,403 - default filter by ffuf
2. Response size `-ms` `-fs` --> less than, greater than, equal to
3. Amount of words `-mw`  `-fw`--> 
4. Specific word match --> regex --> `-mr` `-fr`

- The filtering options can be viewed with the help command `ffuf -h`

`ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb' -fs 900`

- The `-fs` option indicates filter response size with 900

![[Screenshot from 2023-06-06 21-32-05.png]]

- Upon finding the target Vhost in the above case it would be `admin.academy.htb` we can visit the link to confirm that the Vhost indeed exists.

>[!Note]
>There are some checks that need to be done
>1. Don't forget to add "admin.academy.htb" to "/etc/hosts".

- We can perform a recursive scan on the newly found target vhost to see if there are any other subdomains available under the same vhost.