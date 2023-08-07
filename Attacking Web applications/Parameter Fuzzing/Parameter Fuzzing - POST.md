
***
###### `Title :- Parameter Fuzzing - POST`
###### `Created on :- 2023-06-06 - 23:31`
###### `Created by:- Prem J`
***

- POST requests are not passed through url they are passed through the **data** field in  the HTTP request. They cannot be simply appended to the request after `?` similar to GET.
- To fuzz the data field with ffuf, we can use the **-d flag**, as we saw previously in the output of ffuf -h for header. We also have to **add -X POST** to send POST requests.

>[!tip]
>The usage of the data field can differ from different languages, some may have the post data in the request header, by which we may need to fuzz the request header's specific field.
>
>In PHP, "POST" data "content-type" can only accept "application/x-www-form-urlencoded". So, we can set that in "ffuf" with "-H 'Content-Type: application/x-www-form-urlencoded'".

`ffuf -w /opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx`

- above is the command for fuzzing the parameters with post request using the data field and header field

![[Screenshot from 2023-06-06 23-37-11.png]]

- As we can see this time, we got a couple of hits, the same one we got when fuzzing `GET` and another parameter, which is `id`. Let's see what we get if we send a `POST` request with the `id` parameter. We can do that with `curl`, as follows:

`curl http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=key' -H 'Content-Type: application/x-www-form-urlencoded'`

```shell-session
<div class='center'><p>Invalid id!</p></div>
<...SNIP...>
```

- As we can see, the message now says `Invalid id!`.
