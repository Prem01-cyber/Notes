
***
###### `Title :- Gobuster`
###### `Created on :- 2023-05-29 - 20:22`
###### `Created by:- Prem J`
***
#### `Brief -->`

- Gobuster is a tool that we can use to perform **subdomain enumeration**. It is especially interesting for us the patterns options as we have learned some naming conventions from the passive information gathering we can use to discover new subdomains following the same pattern.
- Alternatives are dirbuster (Gui kind and comparatively slow when compared to ffuf ), and ffuf (Very fast and one of the best)

#### `How to -->`

- We can use a wordlist from [Seclists](https://github.com/danielmiessler/SecLists) repository along with `gobuster` if we are looking for words in patterns instead of numbers. Remember that during our passive subdomain enumeration activities, we found a pattern `lert-api-shv-{NUMBER}-sin6.facebook.com`. We can use this pattern to discover additional subdomains. The first step will be to create a **patterns.txt** file with the patterns previously discovered, for example:
- Based on the above patterns the patterns.txt file will include something like

>`lert-api-shv-{GOBUSTER}-sin6`
>`atlas-pp-shv-{GOBUSTER}-sin6`

- The next step will be to launch `gobuster` using the `dns` module, specifying the following options:

- `dns`: Launch the DNS module
- `dir`: directory fuzzer - subdomain enumeration
- `-q`: Don't print the banner and other noise.
- `-r`: Use custom DNS server
- `-d`: A target domain name
- `-p`: Path to the patterns file
- `-w`: Path to the word list
- `-o`: Output file
- `-x`: specifying extensions

- so the final command will be

>`gobuster dns -q -r "${NS}" -d "${TARGET}" -w "${WORDLIST}" -p ./patterns.txt -o "gobuster\_${TARGET}.txt"`

![[Screenshot from 2023-05-29 20-27-11.png]]
- Now we can see a list of subdomains appearing while gobuster is performing the enumeration checks