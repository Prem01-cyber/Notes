
***
###### `Title :- Deobfuscation`
###### `Created on :- 2023-06-09 - 21:44`
###### `Created by:- Prem J`
***

- there are tools to **beautify** and **deobfuscate** the code automatically. 

#### `Beautify -->`

- We have seen [[Basic Obfuscation#`Minifying JavaScript Code -->`]] which has all the code in single line.
- We use Beautify to properly format the code. To achieve we use Beautify. There are several ways to achieve this 
	1. Browser Dev Tools - Browser Debugger - { } -> button - For Pretty print
	2. [Prettier](https://prettier.io/playground/)
	3. [Beautifier](https://beautifier.io/)

>[!Note]
>If the code is only Minified this process would be enough, but in some cases the code would be minified and obfuscated. In such cases we might want some tools to deobfuscate the code.

#### `Debfuscate -->`

- We can find many good online tools to deobfuscate JavaScript code and turn it into something we can understand. One good tool is [JSNice](http://www.jsnice.org/),  **Click on Nicify JavaScript Button to Deobfucate**.

>[!tips]
>We should click on the options button next to the "Nicify JavaScript" button, and de-select "Infer types" to reduce cluttering the code with comments.
>
>Ensure you do not leave any empty lines before the script, as it may affect the deobfuscation process and give inaccurate results.

>[!Note]
> if method used for obfuscation is `packing`. Another way of `unpacking` such code is to find the `return` value at the end and use `console.log` to print it instead of executing it.

#### `Reverse Engineering -->`

- All the above mentioned tools do the process of cleaning the obfuscated code and give us a clean code which is readable.
- But once the code gets more obfuscated and encoded, it would be much more difficult for the automated tools to clean up the code, This is true if the obfuscated tool is **Custom Made**.
- This is where we need to **Manually Reverse Engineer** code to understand how it was obfuscated and its functionality.