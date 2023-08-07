
***
###### `Title :- Code Obfuscation`
###### `Created on :- 2023-06-08 - 06:58`
###### `Created by:- Prem J`
***

#### `What is obfuscation -->`

- Obfuscation is a technique used to make a script more difficult to read by humans but allows it to function the same from a technical point of view, though performance may be slower.
- Usually done by tools, which takes code as input and complex code as output.
- the alternate process for this is [[Deobfuscation]]

>[!Note]
>Codes written in many languages are published and executed without being compiled in `interpreted` languages, such as `Python`, `PHP`, and `JavaScript`. While `Python` and `PHP` usually reside on the server-side and hence are hidden from end-users, `JavaScript` is usually used within browsers at the `client-side`, and the code is sent to the user and executed in cleartext. This is why obfuscation is very often used with `JavaScript`

- This method is used to hide original code and it's functions to prevent it from being reused or copied without developer's permission, difficult to reverse engineer the code's original functionality. Provide a security layer when dealing with authentication or encryption to prevent attacks on vuln's that may be found within the code
- The most common usage of obfuscation, however, is for malicious actions. It is common for attackers and malicious actors to obfuscate their malicious scripts to prevent Intrusion Detection and Prevention systems from detecting their scripts

>[!Danger]
>It must be noted that doing authentication or encryption on the client-side is not recommended, as code is more prone to attacks this way.



