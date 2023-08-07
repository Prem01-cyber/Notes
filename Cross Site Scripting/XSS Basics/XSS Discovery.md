
***
###### `Title :- XSS Discovery`
###### `Created on :- 2023-06-11 - 07:38`
###### `Created by:- Prem J`
***

- In web application vulnerabilites, detecting them can become as difficult as exploiting them. However, as XSS vulnerabilites are widespread, many tools can help us in detecting and identifying them.
- The best method for xss discovery is ***code review***.

#### `Automated Discovery -->`

- There are many web application vulnerability scanners (like [Nessus](https://www.tenable.com/products/nessus), [Burp Pro](https://portswigger.net/burp/pro), or [ZAP](https://owasp.org/www-project-zap/)) have various capabilities for detecting all three types of XSS vulnerabilities.
- These scanners usually do two types of scanning: A Passive Scan, which **reviews client-side code for potential DOM-based vulnerabilities**, and an Active Scan, which **sends various types of payloads to attempt to trigger an XSS through payload injection** in the page source.
- Paid tools have more accuracy than open-source tools but they perform a decent scan
- Some of the common open-source tools that can assist us in XSS discovery are [XSS Strike](https://github.com/s0md3v/XSStrike), [Brute XSS](https://github.com/rajeshmajumdar/BruteXSS), and [XSSer](https://github.com/epsylon/xsser).

>[!Note]
>after cloning XSS strike we go into the directory we use the executable to run the tool.
>`python xssstrike -u "url with params"`



>[!tip]
>We should never trust the results offered by the tools we need to verify them and then confirm it.

#### `Manual Discovery -->`

- When it comes to manual discovery the discovery in finding the vulnerability depends upon the security level of the web application. Basic XSS vulnerabilites can be found easily by testing various XSS payloads, but identifying advanced XSS vulnerabilities requires advanced code review skills.
- We can find huge lists of XSS payloads online, like the one on [PayloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XSS%20Injection/README.md) or the one in [PayloadBox](https://github.com/payloadbox/xss-payload-list)

>[!Note]
>XSS can be injected into any input in the HTML page, which is not exclusive to HTML input fields, but may also be in HTTP headers like the Cookie or User-Agent (i.e., when their values are displayed on the page).

