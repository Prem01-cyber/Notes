
***
###### `Title :- Sub Domain Fuzzing`
###### `Created on :- 2023-06-06 - 07:46`
###### `Created by:- Prem J`
***

- We can also use **ffuf** for sub domain fuzzing.

#### `Sub Domains -->`

- A sub-domain is any website underlying another domain. For example, `https://photos.google.com` is the `photos` sub-domain of `google.com`.
- In this case, we are simply checking different websites to **see if they exist** by checking if they have a public DNS record that would redirect us to a working server IP

`ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u https://FUZZ.hackthebox.eu/ `

- Upon running the command we might get a few hits, upon visiting the links we can confirm they are actual subdomains to the target.

>[!tip]
>Now, we can try running the same thing on `academy.htb` and see if we get any hits back:
>We see that we do not get any hits back. Does this mean that there are no sub-domain under `academy.htb`? - No.
>This means that there are no `public` sub-domains under `academy.htb`, as it does not have a public DNS record, as previously mentioned. Even though we did add `academy.htb` to our `/etc/hosts` file, (in [[DNS Records]]) we only added the main domain, so when `ffuf` is looking for other sub-domains, it will not find them in `/etc/hosts`, and will ask the public DNS, which obviously will not have them.



