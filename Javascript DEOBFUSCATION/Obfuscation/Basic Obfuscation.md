
***
###### `Title :- Basic Obfuscation`
###### `Created on :- 2023-06-08 - 07:23`
###### `Created by:- Prem J`
***

- Obfuscation is usually not done manually, there are many tools that exist for various languages that.
- although obfuscation is possible there are many deobfuscation tools too.

#### `Running JS code -->`

```javascript
console.log('HTB JavaScript Deobfuscation Module');
```

- We can use [JSConsole](https://jsconsole.com/) to run JavaScript code.

#### `Minifying JavaScript Code -->`

- `Code minification` means having the entire code in a **single (often very long) line**, this decreases the readability.
- `Code minification` is more useful for longer code, as if our code only consisted of a single line, it would not look much different when minified.

>[!Note]
>One of the tools for minification  javascript-minifier [javascript-minifier](https://javascript-minifier.com/), Usually, minified JavaScript code is saved with the extension `.min.js`.

- To make sure the minified code runs we just put the minified code into JSConsole and test it.

#### `Obfuscating JS Code (Packing) -->`

- Now, let us obfuscate our line of code to make it more obscure and difficult to read. First, we will try [BeautifyTools](http://beautifytools.com/javascript-obfuscator.php) to obfuscate our code

```javascript
eval(function(p,a,c,k,e,d){e=function(c){return c};if(!''.replace(/^/,String)){while(c--){d[c]=k[c]||c}k=[function(e){return d[e]}];e=function(){return'\\w+'};c=1};while(c--){if(k[c]){p=p.replace(new RegExp('\\b'+e(c)+'\\b','g'),k[c])}}return p}('5.4(\'3 2 1 0\');',6,6,'Module|Deobfuscation|JavaScript|HTB|log|console'.split('|'),0,{}))
```

- Both the JS code's here are the same. To test it we can run and verify that both results are the same.

>[!Note]
>The above type of obfuscation is known as "packing", which is usually recognizable from the six function arguments used in the initial function "function(p,a,c,k,e,d)".

#### `How Packer does it -->`
 
- A `packer` obfuscation tool usually attempts to convert all words and symbols of the code into a list or a dictionary and then refer to them using the `(p,a,c,k,e,d)` function to re-build the original code during execution. The `(p,a,c,k,e,d)` can be different from one packer to another. However, it usually contains a certain order in which the words and symbols of the original code were packed to know how to order them during execution.

#### `Drawbacks -->`

- While a packer does a great job reducing the code's readability, we can still see its main strings written in cleartext, which may reveal some of its functionality. 

#### `Alternatives -->`

