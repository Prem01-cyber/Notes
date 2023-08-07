
***
###### `Title :- Defacing`
###### `Created on :- 2023-06-12 - 06:55`
###### `Created by:- Prem J`
***

#### `Introduction -->`

- The damage and the scope of an XSS attack depends on the type of XSS, a stored XSS being the most critical, while a DOM-based being less so.
- One of the most common attacks usually used with stored XSS vulnerabilities is website defacing attacks. `Defacing` a website means ***changing its look for anyone who visits the website***.

#### `Defacing Elements -->`

- We can utilize injected JavaScript code (through XSS) to make a web page look any way we like.
- Three HTML elements are usually utilized to change the main look of a web page:

1. Background Colour --> document.body.style.background
2. Background --> document.body.background
3. Page Title --> document.title
4. Page Text --> DOM.innerHTML

#### `Changing Background -->`

```html
<script>document.body.style.background = "#141d2b"</script>
<script>document.body.background = "https://www.hackthebox.eu/images/logo-htb.svg"</script>
<script>document.title = 'HackTheBox Academy'</script>
```

>[!tip]
>Here we set the background color to the default Hack The Box background color. We can use any other hex value, or can use a named color like `= "black"`.

- Once we add one of the above payloads to a vulnerable web page, the changes will get reflected. And it will be persistent if the vulnerability is stored XSS.

#### `Changing Page Text -->`

- When we want to change the text displayed on the web page, we can utilize various JavaScript functions for doing so. For example, we can change the text of a specific HTML element/DOM using the `innerHTML` function:

```javascript
document.getElementById("todo").innerHTML = "New Text"
```

- We can also utilize jQuery functions for more efficiently achieving the same thing or for changing the text of multiple elements in one line (to do so, the `jQuery` library must have been imported within the page source):

```javascript
$("#todo").html('New Text');
```

>[!tip]
>It would be wise to try running our HTML code locally to see how it looks and to ensure that it runs as expected, before we commit to it in our final payload.

#### `Summary -->`

- This is because our injected JavaScript code changes the look of the page when it gets executed, which in this case, is at the end of the source code. If our injection was in an element in the middle of the source code, then other scripts or elements may get added to after it, so we would have to account for them to get the final look we need.