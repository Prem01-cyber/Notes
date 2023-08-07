
***
###### `Title :- Reflected XSS`
###### `Created on :- 2023-06-10 - 23:07`
###### `Created by:- Prem J`
***

- There are two types of `Non-Persistant XSS` vulnerabilities: `Reflected XSS`, which gets processed by the back-end server, and `DOM-based XSS`, which is completely processed on the client-side and never reaches the back-end server.
- These vulnerabilites are ***temporary*** and not persistent, cuz they wont get stored in database unlike [[Stored XSS]]
- `Reflected XSS` vulnerabilities occur when our input reaches the back-end server and gets returned to us without being filtered or sanitized.
- There are many cases in which our entire input might get returned to us, like ***error messages*** or ***confirmation messages***. In these cases, we may attempt using XSS payloads to see whether they execute.

![[xss_reflected_1.jpg]]

- as the input is not sanitized we can use the basic XSS payload to check if this is vulnerable to XSS attacks.
- But the difference comes here, once we refresh the page we no longer see the payload we entered making the payload Non-persistent.

>[!Note]
>`But if the XSS vulnerability is Non-Persistent, how would we target victims with it?`
>This depends on which HTTP request is used to send our input to the server. We can check this through the Firefox `Developer Tools` by clicking [`CTRL+I`] and selecting the `Network` tab. Then, we can put our `test` payload again and click `Add` to send it:
>
>![[xss_reflected_network.jpg]]

- As we can see, the first row shows that our request was a `GET` request. `GET` request sends their parameters and data as part of the URL. So, `to target a user, we can send them a URL containing our payload`. 
- To get the URL, we can copy the URL from the URL bar in Firefox after sending our XSS payload, or we can right-click on the `GET` request in the `Network` tab and select `Copy>Copy URL`. Once the victim visits this URL, the XSS payload would execute:

![[Screenshot from 2023-06-11 00-08-41.png]]
