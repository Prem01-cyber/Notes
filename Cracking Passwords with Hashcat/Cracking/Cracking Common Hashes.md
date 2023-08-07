
***
###### `Title :- Cracking Common Hashes`
###### `Created on :- 2023-07-05 - 20:36`
###### `Created by:- Prem J`
***

#### `Cracking Hash Types -->`

- the creators of `Hashcat` maintain a list of [example hashes](https://hashcat.net/wiki/doku.php?id=example_hashes) most hash modes that `Hashcat` supports
- Some hashes might be common and seen on most engagements, while others are seen very rarely or not at all.

|Hashmode|Hash Name|Example Hash|
|---|---|---|
|0|MD5|8743b52063cd84097a65d1633f5c74f5|
|100|SHA1|b89eaac7e61417341b710b727768294d0e6a277b|
|1000|NTLM|b4b9b02e6f09a9bd760f388b67351e2b|
|1800|sha512crypt $6$, SHA512 (Unix)|$6$52450745$k5ka2p8bFuSmoVT1tzOyyuaREkkKBcCNqoDKzYiJL9RaE8yMnPgh2XzzF0NDrUhgrcLwg78xs1w5pJiypEdFX/|
|3200|bcrypt $2*$, Blowfish (Unix)|$2a$05$LhayLxezLhK1LhWvKxCyLOj0j1u.Kj0jZ0pEmm134uzrQlFvQJLF6|
|5500|NetNTLMv1 / NetNTLMv1+ESS|u4-netntlm::kNS:338d08f8e26de93300000000000000000000000000000000:9526fb8c23a90751cdd619b6cea564742e1e4bf33006ba41:cb8086049ec4736c|
|5600|NetNTLMv2|admin::N46iSNekpT:08ca45b7d7ea58ee:88dcbe4446168966a153a0064958dac6:5c7830315c7830310000000000000b45c67103d07d7b95acd12ffa11230e0000000052920b85f78d013c31cdb3b92f5d765c783030|
|13100|Kerberos 5 TGS-REP etype 23|

#### `Example 1 - Database Dumps -->`

- MD5, SHA1, and bcrypt hashes are often seen in database dumps.
- These hashes may be retrieved following a successful SQL injection attack or found in publicly available password data breach database dumps.
- MD5 and SHA1 are typically easier to crack than bcrypt, which may have many rounds of the Blowfish algorithm applied.

`hashcat -a 0 -m 100 hash /opt/.../rockyou.txt`

#### `Example 2 - Linux Shadow File -->`

- Sha512crypt hashes are commonly found in the `/etc/shadow` file on Linux systems. It contains password hashes for all accounts in the system.

```shell-session
root:$6$tOA0cyybhb/Hr7DN$htr2vffCWiPGnyFOicJiXJVMbk1muPORR.eRGYfBYUnNPUjWABGPFiphjIjJC5xPfFUASIbVKDAHS3vTW1qU.1:18285:0:99999:7:::
```

- `$6$` indicates a SHA512 hash and rest of the columns determine the password age and stuff.

```shell-session
hashcat -a 0 -m 1800 hash /opt/.../rockyou.txt
```

#### `Example 3 - Common Active Directory Password Hash -->`

- Credential theft and password re-use are widespread tactics during assessments against organizations using Active Directory to manage their environment
- It is often possible to obtain credentials in cleartext or re-use password hashes to further access via Pass-the-Hash or SMB Relay attacks.+

