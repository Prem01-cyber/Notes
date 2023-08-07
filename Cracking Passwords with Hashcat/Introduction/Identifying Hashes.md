
***
###### `Title :- Identifying Hashes`
###### `Created on :- 2023-06-27 - 19:28`
###### `Created by:- Prem J`
***

- Most hashing algorithms produce hashes of a constant length. The length of a particular hash can be used to map it to the algorithm it was hashed with. 
- For example, a hash of 32 characters in length can be an MD5 or NTLM hash.

>[!tip]
>Sometimes, hashes are stored in certain formats. For example, `hash:salt` or `$id$salt$hash`.

- The following list contains some ids and their corresponding algorithms.

```shell-session
$1$  : MD5
$2a$ : Blowfish
$2y$ : Blowfish, with correct handling of 8 bit characters
$5$  : SHA256
$6$  : SHA512
```

- Open and closed source software use many different kinds of hash formats. 
- For example, the `Apache` web server stores its hashes in the format `$apr1$71850310$gh9m4xcAn3MGxogwX/ztb.`, while `WordPress` stores hashes in the form `$P$984478476IagS59wHZvyQMArzfx58u.`.

#### `Hashid -->`

- [Hashid](https://github.com/psypanda/hashID) is a `Python` tool, which can be used to detect various kinds of hashes. It can be used to identify over 200 hashes.
- The full list of supported hashes can be found [here](https://github.com/psypanda/hashID/blob/master/doc/HASHINFO.xlsx). It can be installed using `pip` - `pip install hashid`
- Hashes can be supplied as command-line arguments or using a file.

```shell-session
prem@htb[/htb]$ hashid '$apr1$71850310$gh9m4xcAn3MGxogwX/ztb.'

Analyzing '$apr1$71850310$gh9m4xcAn3MGxogwX/ztb.'
[+] MD5(APR) 
[+] Apache MD5
```

- If known, `hashid` can also provide the corresponding `Hashcat` hash mode with the `-m` flag if it is able to determine the hash type.

```shell-session
htb[/htb]$ hashid '$DCC2$10240#tom#e4e938d12fe5974dc42a90120bd9c90f' -m
Analyzing '$DCC2$10240#tom#e4e938d12fe5974dc42a90120bd9c90f'
[+] Domain Cached Credentials 2 [Hashcat Mode: 2100
```

#### `Context is Important -->`

- It is not always possible to identify the algorithm based on the obtained hash. Depending on the software, the plaintext might undergo multiple encryption rounds and salting transformations, making it harder to recover.
- It is important to note that `hashid` ***uses regex*** to make a best-effort determination for the type of hash provided
- Often `hashid` will give many possibilities we will be certainly left with a certain amount of guesswork to identify a given hash.
- Was it obtained via an Active Directory attack or from a Windows host?
- Was it obtained through the successful exploitation of a SQL injection vulnerability?
- Knowing where the hash came will help us narrow down the hash type at the end, this is where `Hashcat` comes into play.
- `Hashcat` provides an excellent [reference](https://hashcat.net/wiki/doku.php?id=example_hashes), which maps hash modes to example hashes
- This reference is invaluable during a penetration test to determine the type of hash we are dealing with and the associated hash mode required to pass it to `Hashcat`.

#### `Summary -->`

- We pass the acquired hash with the `hashid`, by which we will obtain the possibilities of the passed hash.
- However a quick look at `Hashcat` example hashes reference will help us determine the hash.
- Context is important during assessments, and is important to consider when working with any tool that attempts to identify hashes. 
