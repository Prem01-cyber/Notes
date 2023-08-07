
***
###### `Title :- DNS Records`
###### `Created on :- 2023-06-06 - 07:37`
###### `Created by:- Prem J`
***

- When we try to access some websites like **academy.htb:PORT** we might some info like,
![[web_fnb_cant_connect_academy.jpg]]
- This is because the exercises we do are not public websites that can be accessed by anyone but local websites within HTB. 
- Browsers only understand how to go to IPs, and if we provide them with a URL, they try to map the URL to an IP by looking into the local `/etc/hosts` file and the public DNS `Domain Name System`

>[!Note]
>If we visit the IP directly, the browser goes to that IP directly and knows how to connect to it.

- But in this case, we tell it to go to `academy.htb`, so it looks into the local `/etc/hosts` file and doesn't find any mention of it.
- It asks the public DNS server about it like Google's DNS 8.8.8.8 and does not find a mention of it. as it is not a public website and eventually fails to connect.
- So to connect to the website we add the entry of the website in the /etc/hosts file.

`sudo sh -c 'echo "SERVER_IP  academy.htb" >> /etc/hosts`

- if we enter the server ip directly we would end up in the same domain, but as we linked with domain name, we can also enter the domain name to reach the target.