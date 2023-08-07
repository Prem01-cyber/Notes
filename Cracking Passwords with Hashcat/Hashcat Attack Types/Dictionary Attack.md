
***
###### `Title :- Dictionary Attack`
###### `Created on :- 2023-06-27 - 20:44`
###### `Created by:- Prem J`
***

- Hashcat has 5 different modes that depends on the complexity of the password.
- The most straightforward but ***extremely effective*** attack type is the ***dictionary attack***.

> [!Note]
> We can also find large wordlists such as [CrackStation's Password Cracking Dictionary](https://crackstation.net/crackstation-wordlist-password-cracking-dictionary.htm), which contains 1,493,677,782 words and is 15GB in size, Depending on the requirement there are wordlists in 40GB size

#### `Straight or Dictionary Attack -->`

- Hashcat reads from the wordlist provided and tries to crack the hashes.
- Dictionary Attacks are useful if the target organisations use ***weak passwords***

```shell-session
htb[/htb]$ hashcat -a 0 -m <hash type> <hash file> <wordlist>
```

- Above is the syntax for a Dictionary attack using hashcat.
- Cracking speed varies depending on the underlying hardware, hash type, and complexity of the password.

>[!Note]
>A more complex hash such as [Bcrypt](https://en.wikipedia.org/wiki/Bcrypt), which is a type of password hash based on the [Blowfish](https://en.wikipedia.org/wiki/Blowfish_(cipher)) cipher

#### `Summary -->`

- Dictionary attacks can we very effective against weak passwords, but the attacks efficiency also depends upon the type of hash targeted.

