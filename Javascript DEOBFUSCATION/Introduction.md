
***
###### `Title :- Introduction`
###### `Created on :- 2023-06-08 - 00:03`
###### `Created by:- Prem J`
***

- It is an important skill to master code analysis and reverse engineering.
- Obfuscation refers to **make something less clear and more difficult to understand**.
- we often come across obfuscated code that wants to hide certain functionalities, like malware that utilizes obfuscated JavaScript code to retrieve its main payload.

Java Script --> to perform their functions that are necessary to run website
HTML --> determine website's main fields and parameters
CSS --> design

- Source code is available on the client-side, it is rendered by our browsers, to take a peek at the rendered source code `ctrl + u`

Java Script --> between `<script>...</script>` or `<script src="secret.js"></script>`
CSS --> between `<style>...</style>` or `<link rel="stylesheet" href="style.css"\>`

- We can check out the script by clicking on `secret.js`, which should take us directly into the script. When we visit it, we see that the code is very complicated and cannot be comprehended:

````javascript
eval(function (p, a, c, k, e, d) { e = function (c) { '...SNIP... |true|function'.split('|'), 0, {}))
````

