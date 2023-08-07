
***
###### `Title :- Hybrid Mode (Brute + Mask) (Mask + Brute)`
###### `Created on :- 2023-06-28 - 06:24`
###### `Created by:- Prem J`
***

- It is a variation of combinator attack, where multiple modes can be used together for a fine-tuned wordlist creation.
- This mode can be used to perform very targeted attacks by creating very customized wordlists.
- It is particularly useful when you know or have a general idea of the organization's password policy or common password syntax. 
- The attack mode for the hybrid attack is "`6`".

```shell-session
hashcat -a 6 -m 0 hybrid_hash /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt '?d?s'
```

- Hashcat reads words from the wordlist ([[Dictionary Attack]]) and appends a unique string based on the mask supplied ([[Mask Attack]])
- Attack mode 6 - Appends the mask to the wordlist, Attack mode 7 - prepend's the mask to the wordlist

```shell-session
hashcat -a 7 -m 0 hybrid_hash_prefix -1 01 '20?1?d' /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt
```




