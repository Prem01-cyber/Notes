
***
###### `Title :- Basic Hash Cracking`
###### `Created on :- 2023-08-09 - 20:56`
###### `Created by:- Prem J`
***

- `john [options] [path to file]` -> path to file is the file containing the hash you want to crack
- `john --wordlist=[path to wordlist] [path to file]`
- `john --format=[format] --wordlist=[path to wordlist] [path to file]` -> format specific hashing. for instance, `john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt`
- When you are telling john to use formats, if you're dealing with a standard hash type, e.g. `md5` as in the example above, you have to prefix it with`raw-` to tell john you're just dealing with a standard hash type, though this doesn't always apply. 
- To check if you need to add the prefix or not, you can list all of John's formats using `john --list=formats` and either check manually, or grep for your hash type using something like `john --list=formats | grep -iF "md5"`.  


>[!Note]
>we find the hash using tools available online or some of your own tools. For example, [hash-identifier](https://gitlab.com/kalilinux/packages/hash-identifier/-/tree/kali/master)



