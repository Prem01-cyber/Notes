
***
###### `Title :- Running SQLMap on an HTTP Request`
###### `Created on :- 2023-06-22 - 06:18`
###### `Created by:- Prem J`
***

- SQLMap has numerous options and switches that can be used to properly set up the (HTTP) request before its usage.

>[!tip]
>`sqlmap ... --batch` this option allows sqlmap to use default options automatically

>[!Danger]
>In many cases, simple mistakes such as forgetting to provide proper cookie values, over-complicating setup with a lengthy command line, or improper declaration of formatted POST data, will prevent the correct detection and exploitation of the potential SQLi vulnerability.

#### `Curl Commands -->`

- The best way to properly setup SQLMap request against the specific target (Web request with parameters) is by utilizing `Copy as curl` from network tab.

![[Pasted image 20230622063211.png]]

`curl 'http://138.68.141.178:30085/case1.php?id=1' -H 'User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:102.0) Gecko/20100101 Firefox/102.0' -H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8' -H 'Accept-Language: en-US,en;q=0.5' -H 'Accept-Encoding: gzip, deflate' -H 'Referer: http://138.68.141.178:30085/case1.php' -H 'DNT: 1' -H 'Connection: keep-alive' -H 'Upgrade-Insecure-Requests: 1' -H 'Sec-GPC: 1'`

- Above is the copy of the `copy as curl` excluding the curl command rest is passed with sqlmap to perform SQLi injection enumeration.

```shell-session
prem@htb[/htb]$ sqlmap 'http://www.example.com/?id=1' -H 'User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0' -H 'Accept: image/webp,*/*' -H 'Accept-Language: en-US,en;q=0.5' --compressed -H 'Connection: keep-alive' -H 'DNT: 1'
```

- When providing data for testing to SQLMap, there has to be wither a parameter value that could be assessed for SQLi vulnerability or ***specialised options or switches for automatic parameter finding*** (--crawl, --forms or -g)

#### `GET or POST Requests -->`

- In the most common scenario, `GET` parameters are provided with the usage of option `-u`/`--url`, as in the previous example. As for testing `POST` data, the `--data` flag can be used. Method should also be specified for better results.

```shell-session
premjampuram@htb[/htb]$ sqlmap -u 'http://www.example.com/' --method POST --data 'uid=1&name=test'
```

- if we know `uid` is vulnerable then we can specify only `uid` for sqlmap

```shell-session
premjampuram@htb[/htb]$ sqlmap 'http://www.example.com/' --method POST --data 'uid=1&name=test' -p uid
```

#### `Full HTTP Requests -->`

- If there are lots of header files that need to be supplied to SQLMap then we use the `-r` flag to specify the request file itself. Which can be captured through [[burpsuite]]

![[Pasted image 20230622065032.png]]

```shell-session
premjampuram@htb[/htb]$ sqlmap -r req.txt
```

>[!tip]
>similarly to the case with the '--data' option, within the saved request file, we can specify the parameter we want to inject in with an asterisk (*), such as '/?id=*'.

- Apart from the most common form-data `POST` body style (e.g. `id=1`), SQLMap also supports JSON formatted (e.g. `{"id":1}`) and XML formatted (e.g. `<element><id>1</id></element>`) HTTP requests.
- This JSON text can be obtained through burpsuite.

```shell-session
premjampuram@htb[/htb]$ cat req.txt
HTTP / HTTP/1.0
Host: www.example.com

{
  "data": [{
    "type": "articles",
    "id": "1",
    "attributes": {
      "title": "Example JSON",
      "body": "Just an example",
      "created": "2020-05-22T14:56:29.000Z",
      "updated": "2020-05-22T14:56:28.000Z"
    },
    "relationships": {
      "author": {
        "data": {"id": "42", "type": "user"}
      }
    }
  }]
}
```

#### `Custom SQLMap Requests -->`

- If we wanted to craft complicated requests manually, there are numerous switches and options to fine-tune SQLMap.

cookie mentioning options --> `-cookie='VALUE'` alt `-H='Cookie:VALUE'`

![[Screenshot from 2023-06-22 21-08-20.png]]

- We can apply the same to options like `--host`, `--referer`, and `-A/--user-agent`, which are used to specify the same HTTP headers' values.
- there is a switch `--random-agent` designed to randomly select a `User-agent` header value from the included database of regular browser values.
- This is an important switch to remember, as more and more protection solutions automatically drop all HTTP traffic containing the recognizable default SQLMap's User-agent value (e.g. `User-agent: sqlmap/1.4.9.12#dev (http://sqlmap.org)`). Alternatively, the `--mobile` switch can be used to imitate the smartphone by using that same header value.

>[!Note]
>While SQLMap, by default, targets only the HTTP parameters, it is possible to test the headers for the SQLi vulnerability. The easiest way is to specify the "custom" injection mark after the header's value (e.g. `--cookie="id=1*"`). The same principle applies to any other part of the request

- Also, if we wanted to specify an alternative HTTP method, other than `GET` and `POST` (e.g., `PUT`), we can utilize the option `--method`.

```shell-session
prem@htb[/htb]$ sqlmap -u www.target.com --data='id=1' --method PUT
```



