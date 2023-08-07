
***
###### `Title :- Introduction`
###### `Created on :- 2023-06-24 - 07:23`
###### `Created by:- Prem J`
***

#### `What is Brute Forcing -->`

- A [Brute Force](https://en.wikipedia.org/wiki/Brute-force_attack) attack is a method of guessing the password bluntly by using ***automatic probing***. An example of brute forcing is password cracking.
- In general passwords are not stored in clear text.
- Here is a small list of files that can contain hashed passwords:

|**`Windows`**|**`Linux`**|
|---|---|
|unattend.xml|shadow|
|sysprep.inf|shadow.bak|
|SAM|password|

#### `How does it work -->1`

- The hashes are made are one way hash meaning they cannot be reversed. So the password cannot be calculated backwards.
- The brute forcing method determines the hash values belonging to the randomly selected passwords until a hash value matches indicating that it might be the password.
- This method is also called as ***offline brute forcing***.

#### `Tools Used -->`

- There are many tools and methods to utilize for login brute-forcing, like:
	- Ncrack
	- wfuzz`
	- medusa
	- patator
	- hydra
	- and others.
- Among the above tools ***Hydra*** is deemed to be one of the best to brute force login forms.