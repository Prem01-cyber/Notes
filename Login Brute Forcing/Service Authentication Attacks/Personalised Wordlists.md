
***
###### `Title :- Personalised Wordlists`
###### `Created on :- 2023-06-26 - 07:03`
###### `Created by:- Prem J`
***

- In order to create a personalised wordlist we need to gather some information about them. 

#### `CUPP -->`

- Many tools can create wordlists, one of the tools is the `CUPP`. we can install cupp through `sudo apt install cupp` or clone it from the [Github repository](https://github.com/Mebus/cupp).
- It is one of he easier tools to use and we can add `-i` flag to make the tool interactive.
- It generates the wordlist based on the information we gathered

#### `Password Policy -->`

- Upon generating the wordlist we can filter out them based on our own requirements and generate a new wordlist. The requirements can be like
	
	1. 8 characters or longer
	2. contains special characters
	3. contains numbers

- Some tools would convert password policies to `Hashcat` or `John` rules, but `hydra`does not support rules for filtering passwords. So, we will simply use the following commands to do that for us:

```bash
sed -ri '/^.{,7}$/d' william.txt            # remove shorter than 8
sed -ri '/[!-/:-@\[-`\{-~]+/!d' william.txt # remove no special chars
sed -ri '/[0-9]+/!d' william.txt            # remove no numbers
```

#### `Mangling -->`

- It is still possible to create many permutations of each word in that list. We never know how our target thinks when creating their password, and so our safest option is to add as many alterations and permutations as possible, noting that this will, of course, take much more time to brute force.
- Many great tools do word mangling and case permutation quickly and easily, like [rsmangler](https://github.com/digininja/RSMangler) or [The Mentalist](https://github.com/sc0tfree/mentalist.git). 
- These tools have many other options, which can make any small wordlist reach millions of lines long. 

>[!tip]
>The more mangled a wordlist is, the more chances you have to hit a correct password, but it will take longer to brute force. So, always try to be efficient, and properly customize your wordlist using the intelligence you gathered.

#### `Custom Username wordlist -->`

- We should also consider creating a personalised username wordlist based on the person's available details. 
- For example, the person's username could be `b.gates` or `gates` or `bill`, and many other potential variations. There are several methods to create the list of potential usernames, the most basic of which is simply writing it manually.
- One such tool we can use is [Username Anarchy](https://github.com/urbanadventurer/username-anarchy), which we can clone from GitHub.
- This tool has many use cases that we can take advantage of to create advanced lists of potential usernames. However, for our simply use case, we can simply run it and provide the first/last names as arguments

```bash
./username-anarchy Bill Gates > bill.txt
```

