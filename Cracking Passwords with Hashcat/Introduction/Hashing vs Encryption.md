
***
###### `Title :- Hashing vs Encryption`
###### `Created on :- 2023-06-27 - 06:57`
###### `Created by:- Prem J`
***

#### `Hashing -->`

- Is the process of converting some text to a string, which is unique to that particular text.
- Usually, a hash function always returns hashes with the same length irrespective of the type, length, or size of the data.
- The hashing is a one-way process. These hashes can be used for various purposes like

[MD5](https://en.wikipedia.org/wiki/MD5) and [SHA256](https://en.wikipedia.org/wiki/SHA-2) --> Verify File Integrity
[PBKDF2](https://en.wikipedia.org/wiki/PBKDF2) --> used to hash passwords before storage

>[!tip]
>```shell-session
>premjampuram@htb[/htb]$ echo -n "p@ssw0rd" | md5sum
>0f359740bd1cda994f8b55330c86d845
>```

- Some hash functions can be keyed i.e additional secret used to create the hash.

 [HMAC](https://en.wikipedia.org/wiki/HMAC) --> acts as a [checksum](https://en.wikipedia.org/wiki/Checksum) to verify if a particular message was tampered with during transmission

- mainly four different algorithms can be used to protect passwords on Unix systems. These are `SHA-512`, `Blowfish`, `BCrypt`, and `Argon2`

- `SHA-512` converts a long string of characters into a hash value. It is fast and efficient, but there are many rainbow table attacks where an attacker uses a pre-computed table to reconstruct the original passwords.
- `Blowfish` is a symmetric block cipher algorithm that encrypts a password with a key. It is more secure than SHA-512 but also a lot slower.
- `BCrypt` uses a slow hash function to make it harder for potential attackers to guess passwords or perform rainbow table attacks.
- `Argon2` is a modern and secure algorithm explicitly designed for password hashing systems. It uses multiple rounds of hash functions and a large amount of memory to make it harder for attackers to guess passwords

- Among the four `Argon2` is the most secured algorithm as it has high `time` and `resource` requirement.
- One protection employed against the brute-forcing of hashes is ***salting***. A salt is a random piece of data added to the plaintext before hashing it. This increases the computation time but does not prevent brute force altogether.

>[!Note]
>Some hash functions such as MD5 have also been vulnerable to collisions, where two sets of plaintext can produce the same hash.

#### `Encryption -->`

- Encryption is the process of converting data into a format in which the original content is not accessible.
- Unlike hashing, encryption is reversible, i.e., it's possible to decrypt the ciphertext (encrypted data) and obtain the original content.
- Some classic examples of encryption ciphers are the [Caesar cipher](https://en.wikipedia.org/wiki/Caesar_cipher), [Bacon's cipher](https://en.wikipedia.org/wiki/Bacon%27s_cipher) and [Substitution cipher](https://en.wikipedia.org/wiki/Substitution_cipher). 
- Encryption algorithms are of two types

>[!Note]
>`python`
>`from pwn import xor`
>`xor("opens3same","academy")`
>`b'\x0e\x13\x04\n\x16^\n\x00\x0e\x04'`
>The above encryption is both way if we reverse the process we will the `opens3same` as decryption answer

##### `1. Symmetric Encryption -->`

- Symmetric algorithms use a key or secret to encrypt the data and use the ***same key*** to decrypt it.
- Some other examples of symmetric algorithms are [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), [DES](https://en.wikipedia.org/wiki/Data_Encryption_Standard), [3DES](https://en.wikipedia.org/wiki/Triple_DES) and [Blowfish](https://en.wikipedia.org/wiki/Blowfish_(cipher)#The_algorithm). These algorithms can be vulnerable to attacks such as key bruteforcing, [frequency analysis](https://en.wikipedia.org/wiki/Frequency_analysis), [padding oracle attack](https://en.wikipedia.org/wiki/Padding_oracle_attack), etc.

##### `2. Asymmetric Encryption -->`

- On the other hand, Asymmetric algorithms divide the key into two parts (i.e., ***public and private***). The public key can be given to anyone who wishes to encrypt some information and pass it securely to the owner. The owner then uses their private key to decrypt the content.
- Some examples of asymmetric algorithms are [RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)), [ECDSA](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm), and [Diffie-Hellman](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange)

- One of the prominent uses of asymmetric encryption is the `Hypertext Transfer Protocol Secure` (`HTTPS`) protocol in the form of `Secure Sockets Layer`(`SSL`).
- When a client connects to a server hosting an `HTTPS` website, a public key exchange occurs. The client's browser uses this public key to encrypt any kind of data sent to the server. 
- The server decrypts the incoming traffic before passing it on to the processing service.