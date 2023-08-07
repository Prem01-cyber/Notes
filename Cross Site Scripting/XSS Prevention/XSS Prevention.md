
***
###### `Title :- XSS Prevention`
###### `Created on :- 2023-06-13 - 12:56`
###### `Created by:- Prem J`
***

- As discussed previously, XSS vulnerabilities are mainly linked to two parts of the web application: A `Source` like a user input field and a `Sink` that displays the input data. there are two main points that we should focus on securing, both in the front-end and in the back-end.

### `Front End -->`

- the main aspect of XSS prevention is ***input sanitization*** and ***input validation***

Front End --> Place where Input validation and sanitization must be done mainly

- Input validation refers to allowing specific details only on specific fields Ex: invalid emails 
- Input sanitization refers to not allowing any input with JavaScript Code in it, by escaping any special characters. For this, we can utilize the [DOMPurify](https://github.com/cure53/DOMPurify)JavaScript library, as follows:

```javascript
<script type="text/javascript" src="dist/purify.min.js"></script>
let clean = DOMPurify.sanitize( dirty );
```

- This will escape any special characters with a backslash `\`, which should help ensure that a user does not send any input with special characters (like JavaScript code), which should prevent vulnerabilities like DOM XSS.

#### `direct input -->`

- Finally, we should always ensure that we never use user input directly within certain HTML tags, like:

1. JavaScript code `<script></script>`
2. CSS Style Code `<style></style>`
3. Tag/Attribute Fields `<div name='INPUT'></div>`
4. HTML Comments `<!-- -->`

- If user input goes into any of the above categories, it can inject malicious JavaScript code.
- In addition to this we should avoid using JavaScript functions that ***allow changing raw text of HTML fields*** like:
	- `DOM.innerHTML`
	- `DOM.outerHTML`
	- `document.write()`
	- `document.writeln()`
	- `document.domain`

- And the following jQuery functions:
	- `html()`
	- `parseHTML()`
	- `add()`
	- `append()`
	- `prepend()`
	- `after()`
	- `insertAfter()`
	- `before()`
	- `insertBefore()`
	- `replaceAll()`
	- `replaceWith()`

- As these functions write ***raw text to the HTML code***, if any user input goes into them, it may include malicious JavaScript code, which leads to an XSS vulnerability.

### `Back End -->`

- ***Input validation and Input sanitization*** also plays a crucial role in backend to prevent XSS vulnerabilites.
- An example of E-Mail validation on a PHP back-end is the following:

```php
if (filter_var($_GET['email'], FILTER_VALIDATE_EMAIL)) {
    // do task
} else {
    // reject input - do not display it
}
```

- front-end input sanitization can be easily bypassed by sending custom `GET` or `POST`requests. Luckily, there are very strong libraries for various back-end languages that can properly sanitize any user input, such that we ensure that no injection can occur.
- For example, for a PHP back-end, we can use the `addslashes` function to sanitize user input by escaping special characters with a backslash:

```php
addslashes($_GET['email'])
```

- For a NodeJS back-end, we can also use the [DOMPurify](https://github.com/cure53/DOMPurify) library as we did with the front-end

#### `output HTML Encoding -->`

- This means that we have to encode any special characters into their HTML codes
- For a PHP back-end, we can use the `htmlspecialchars` or the `htmlentities` functions
- For a NodeJS back-end, we can use any library that does HTML encoding, like `html-entities`
- Once we ensure that all user input is validated, sanitized, and encoded on output, we should significantly reduce the risk of having XSS vulnerabilities

#### `server configuration -->`

- there are certain back-end web server configurations that may help in preventing XSS attacks
	- Using HTTPS across the entire domain.
	- Using XSS prevention headers.
	- Using the appropriate Content-Type for the page, like `X-Content-Type-Options=nosniff`.
	- Using `Content-Security-Policy` options, like `script-src 'self'`, which only allows locally hosted scripts.
	- Using the `HttpOnly` and `Secure` cookie flags to prevent JavaScript from reading cookies and only transport them over HTTPS.

>[!tip]
>In addition to the above, having a good `Web Application Firewall (WAF)` can significantly reduce the chances of XSS exploitation, as it will automatically detect any type of injection going through HTTP requests and will automatically reject such requests. Furthermore, some frameworks provide built-in XSS protection, like [ASP.NET](https://learn.microsoft.com/en-us/aspnet/core/security/cross-site-scripting?view=aspnetcore-7.0).
