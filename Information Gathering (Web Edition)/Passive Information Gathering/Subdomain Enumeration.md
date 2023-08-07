
***
###### `Title :- Subdomain Enumeration`
###### `Created on :- 2023-05-27 - 19:06`
###### `Created by:- Prem J`
***
#### `Brief -->`

- Subdomain enumeration refers to mapping all the subdomains to a domain name.
- We only perform  **passive subdomain enumeration** using some third party [[information_gathering_tools]].

#### `Advantages -->`

- It increases our attack surface and may uncover hidden management backend panels or intranet web applications that network administrators expected to keep hidden using the "**security by obscurity**"[^1] strategy.

#### `How to -->`

- There are many tools that we can use to perform Subdomain Enumeration like **curl**, **openssl**. And some third party resources like **Virus total, ctr.sh, censys.io**
- This passive subdomain enumeration can also be automated using command line tools like **[[theHarvester]]**, 
- we can use [Subdomainfinder](https://subdomainfinder.c99.nl/). 
- We can also use Sublist3r
- Amass is also one of the best tools if we master it

>[!Note]
>another interesting source of information that we can use to extract **certificates** from subdomains is SSL/TLS certificates issued by a **Certificate Authority (CA)** to be published in a publicly accessible log. Some of the resources to extract certificates can be found in [[information_gathering_tools]]

