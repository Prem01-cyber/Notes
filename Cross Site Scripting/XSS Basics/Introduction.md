
***
###### `Title :- Introduction`
###### `Created on :- 2023-06-10 - 10:41`
###### `Created by:- Prem J`
***

- As web applications become more advanced and more common, so do web application vulnerabilities. Among the most common types of web application vulnerabilities are [Cross-Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/) vulnerabilities. 
- XSS vulnerabilities take advantage of a flaw in user **input sanitization** to "write" **JavaScript code** to the page and execute it on the client side, leading to several types of attacks.

#### `What is XSS -->`

- A web application works by receiving the HTML Code from backend server and rendering it to the client side internet browser.
- When a vulnerable web application does not properly sanitize user input, a malicious user can inject extra JavaScript code in an input field.
- So when another user views the same page, they unknowingly execute the malicious JavaScript code.

#### `Risk management -->`

- XSS vulnerabilities are solely executed on the client-side and hence do not directly affect the back-end server. They can only affect the user executing the vulnerability. The direct impact of XSS vulnerabilities on the back-end server may be relatively low, but they are very commonly found in web applications, so this equates to a medium risk (`low impact + high probability = medium risk`), which we should always attempt to `reduce` risk by detecting, remediating, and proactively preventing these types of vulnerabilities.

![[xss_risk_chart_1.jpg]]

#### `XSS Attacks -->`

- A basic example of an XSS attack is having the target user unwittingly send their session cookie to the attacker's web server
- Another example is having the target's browser execute API calls that lead to a malicious action, like changing the user's password to a password of the attacker's choosing
- There are many other types of XSS attacks, from Bitcoin mining to displaying ads.
- As XSS attacks execute JavaScript code within the browser, they are limited to the browser's JS engine (i.e., V8 in Chrome). They cannot execute system-wide JavaScript code to do something like system-level code execution

>[!Note]
>if a skilled researcher identifies a binary vulnerability in a web browser (e.g., a Heap overflow in Chrome), they can utilize an XSS vulnerability to execute a JavaScript exploit on the target's browser, which eventually breaks out of the browser's sandbox and executes code on the user's machine.

#### `Samy Worm -->`

- It is a well known XSS vulnerability, which was a browser-based worm that exploited a stored XSS vulnerability in the social networking website MySpace back in 2005
- It executed when viewing an infected webpage by posting a message on the victim's MySpace page that read, `Samy is my hero.` The message itself also contained the same JavaScript payload to **re-post the same message** when viewed by others. Within a single day, more than a million MySpace users had this message posted on their pages

#### `Types of XSS -->`

1. **[[Stored XSS]]** (Persistent) --> The most critical type of XSS, which occurs when *user input **is stored** on the back-end database* and then displayed upon retrieval (e.g., posts or comments)
2. **[[Reflected XSS]]** (Non-Persistent) --> Occurs when *user input is displayed on the page after being processed by the backend server, but **without being stored*** (e.g., search result or error message)
3. **[[DOM-based XSS]]** --> Another Non-Persistent XSS type that occurs when user input is directly shown in the browser and is ***completely processed on the client-side***, without reaching the back-end server (e.g., through client-side HTTP parameters or anchor tags)