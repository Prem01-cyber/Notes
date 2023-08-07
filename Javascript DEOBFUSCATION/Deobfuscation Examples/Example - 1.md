
***
###### `Title :- Example - 1`
###### `Created on :- 2023-06-09 - 22:15`
###### `Created by:- Prem J`
***

```javascript
'use strict';
function generateSerial() {
  ...SNIP...
  var xhr = new XMLHttpRequest;
  var url = "/serial.php";
  xhr.open("POST", url, true);
  xhr.send(null);
};
```

- secret.js is an deobfuscated code, lets start going through

#### `Code Analysis -->`

- We see that the `secret.js` file contains only one function, `generateSerial`.
- each line is a HTTP Request
- `XMLHttpRequest` Upon googling it, it says that it is a JavaScript function that handles web requests.
- there are some code variables (`xhr`, `url`) and code functions (open, send).
- So, all `generateSerial` is doing is simply sending a `POST` request to `/serial.php`, without including any `POST` data or retrieving anything in return.
- We can use a post request to the `./serial.php` using [[curl]] to find out that if it retrieves anything `curl -s http://server:port/serial.php -X POST -d "key:value"`- 

#### `Decoding -->`

- Upon using curl to request the server, we have received a test that appears to be encoded `N2gxNV8xNV9hX3MzY3IzN19tMzU1NGcz`
- We can use online tools to decode it to see if we can get any important details.

>[!Note]
>This is another important aspect of obfuscation that we referred to in `More Obfuscation` in the `Advanced Obfuscation` section. Many techniques can further obfuscate the code and make it less readable by humans and less detectable by systems. For that reason, you will very often find obfuscated code containing encoded text blocks that get decoded upon execution. We will cover 3 of the most commonly used text encoding methods:
>
>base64
>hex
>rot13

#### `Base64 -->`

- `base64` encoding is usually used to reduce the use of special characters, as any characters encoded in `base64` would be represented in alpha-numeric characters, in addition to `+` and `/` only. Regardless of the input, even if it is in binary format, the resulting base64 encoded string would only use them
- length of base64 encoding results with a string of length in multiples of 4.
- If the resulting output is only 3 characters long, for example, an extra `=` is added as padding, and so on.

`Base64 Encode` --> echo httpd | base64 --> aHR0cGQK - length=8
`Base64 Decode` --> echo aHR0cGQK | base64 -d --> httpd

#### `Hex -->`

- Another common encoding method is `hex` encoding, which encodes each character into its `hex` order in the `ASCII` table.
- For example, `a` is `61` in hex, `b` is `62`, `c` is `63`, and so on. 
- Any string encoded in `hex` would be comprised of hex characters only, which are 16 characters only: 0-9 and a-f. That makes spotting `hex` encoded strings just as easy as spotting `base64` encoded strings.

`Hex Encode` --> echo httpd | xxd -p --> 68747470640a - only 0-9 and a-f
`Hex Decode` --> echo 68747470640a | xxd -p -r --> httpd

#### `Caesar/Rot13 -->`

- Another common -and very old- encoding technique is a **Caesar cipher**, which shifts each letter by a fixed number.
- For example, shifting by 1 character makes `a` become `b`, and `b` becomes `c`, and so on. Many variations of the Caesar cipher use a different number of shifts, the most common of which is `rot13`, which shifts each character 13 times forward
- Even though this encoding method makes any text looks random, it is still possible to spot it because each character is mapped to a specific character.
- For example, in `rot13`, `http://www` becomes `uggc://jjj`, which still holds some resemblances and may be recognised as such.

`Rot13 Encode` --> echo httpd | tr 'A-Za-z' 'N-ZA-Mn-za-m' --> uggcq --> same pattern
`Rot13 Decode` --> echo uggcq | tr 'A-Za-z' 'N-ZA-Mn-za-m' --> httpd

- We can also use [Rot13](https://rot13.com/) for encode/Decode

#### `Other types of Encodings -->`

- There are hundreds of other encoding methods we can find online. Even though these are the most common, sometimes we will come across other encoding methods, which may require some experience to identify and decode.

>[!tip]
>If you face any similar types of encoding, first try to determine the type of encoding, and then look for online tools to decode it.

- Some tools can help automatically determine the type of encoding, like [Cipher Identifier](https://www.boxentriq.com/code-breaking/cipher-identifier) 
- Other than encoding, many obfuscation tools utilize encryption, which is encoding a string using a key, which may make the obfuscated code very difficult to reverse engineer and deobfuscate, especially if the decryption key is not stored within the script itself.
