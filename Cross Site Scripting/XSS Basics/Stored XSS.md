
***
###### `Title :- Stored XSS`
###### `Created on :- 2023-06-10 - 22:33`
###### `Created by:- Prem J`
***

- Among the three the most critical vulnerability is the Stored XSS  cuz if our payload gets stored in the backend database and retrieved upon visiting the page it may affect any user that visits the page.
- This vulnerability targets wider audience. Making it most critical of them all.

#### `XSS Testing Payloads -->`

- We enter some of the JavaScript scripts in the user input area in the website to check if the web server is vulnerable to XSS.

```html
<script>alert(window.origin)</script>
```

- Upon entering the above Basic XSS Payload which just shows an alert box if the web server is vulnerable to it. 

![[xss_stored_xss_alert.jpg]]

- The alert comes because the input entered by the user is not validated and directly added to source code making it vulnerable to XSS attacks. Upon viewing the source code after entering the JavaScript Payload it may look like

```html
<div></div><ul class="list-unstyled" id="todo"><ul><script>alert(window.origin)</script>
</ul></ul>
```

>[!tip]
>Many modern web applications utilize cross-domain IFrames to handle user input, so that even if the web form is vulnerable to XSS, it would not be a vulnerability on the main web application. This is why we are showing the value of `window.origin` in the alert box, instead of a static value like `1`. In this case, the alert box would reveal the URL it is being executed on, and will confirm which form is the vulnerable one, in case an IFrame was being used.

- some browsers may block the `alert()` function in JavaScript so it may be comfortable to know some extra functions that will be useful in Identifying the XSS existence. Some of them are mentioned in [[cheatsheet-103]]

>[!Note]
>To see whether the payload is persistent and stored on the back-end, we can refresh the page and see whether we get the alert again. If we do, we would see that we keep getting the alert even throughout page refreshes, confirming that this is indeed a `Stored/Persistent XSS` vulnerability. This is not unique to us, as any user who visits the page will trigger the XSS payload and get the same alert.

