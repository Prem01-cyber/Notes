
***
###### `Title :- DOM-based XSS`
###### `Created on :- 2023-06-11 - 00:09`
###### `Created by:- Prem J`
***

- While `reflected XSS` sends the input data to the back-end server through HTTP requests, DOM XSS is completely processed on the client-side through JavaScript. DOM XSS occurs when JavaScript is used to change the page source through the `Document Object Model (DOM)`.
- Unlike `reflected XSS` if we open the network tab and observe the network tab we can find that there are no HTTP requests made indicating it is not prone to `reflected XSS` attacks.

![[xss_dom_network.jpg]]

- We see that the input parameter in the URL is using a hashtag `#` for the item we added, which means that this is a client-side parameter that is completely processed on the browser. This indicates that the input is being processed at the client-side through JavaScript and never reaches the back-end; hence it is a `DOM-based XSS`.
- If we look at the page source, we will notice that out `test` string is nowhere to be found. This is because JavaScript is updating the page when we do an action. Which is after the page source is retrieved by our browser.
- Hence base page source will not show our input, if we refresh the page it will not be retained (`Non-persistent`).

![[xss_dom_inspector (1).jpg]]

- Above image is the rendered page upon our action.

#### `Source and Sink -->`

- The `Source` is the JavaScript object that takes the user input, and it can be any input parameter like a URL parameter or an input field, as we saw above.
- the `Sink` is the function that writes the user input to a DOM Object on the page. 
- the `Sink` is the function that writes the user input to a DOM Object on the page. If the `Sink`function does not properly sanitize the user input, it would be vulnerable to an XSS attack.
- Some of the commonly used JavaScript functions to write to DOM objects are:
	- document.write()
	- DOM.innerHTML
	- DOM.outerHTML

- Furthermore, some of the `jQuery` library functions that write to DOM objects are:
	- add()
	- after()
	- append()


#### `XSS Attacks -->`

- If we try the XSS payload we have been using previously, we will see that it will not execute. This is because the `innerHTML` function does not allow the use of the `<script>`tags within it as a security feature

![[Screenshot from 2023-07-13 08-28-07.png]]

```html
<img src="" onerror=alert(window.origin)>
```

- The above line creates a new HTML image object, which has a `onerror` attribute that can execute JavaScript code when the image is not found. So, as we provided an empty image link (`""`), our code should always get executed without having to use `<script>` tags:

![[xss_dom_alert.jpg]]

- To attack the user with DOM vulnerability we copy the url same as in [[Reflected XSS]] and share it to the target, upon clicking the link payloads get executed and target is exploited.