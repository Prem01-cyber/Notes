
***
###### `Title :- Virtual Hosts`
###### `Created on :- 2023-05-29 - 07:17`
###### `Created by:- Prem J`
***
#### `Brief -->`

- A Virtual Host (vHost) is an option which allows several websites to be hosted on a single server. There are two ways to configure virtual hosts

### `1. IP - Based virtual hosting -->`

- A host can have multiple network interfaces. Multiple IP addresses, or interface aliases, can be configured on each network interface of a host. The servers on the host can bind to one or more IP addresses. 
- Different servers can be addressed under different IP addresses on this host. From the client's point of view the servers are independent of each other.

### `2. Name - Based virtual hosting -->`

- The distinction of which domain the service was requested is made at the application level. 

	admin.inlanefreignt.htb -> 192.168.10.10 -> /var/www/admin
	backup.inlanefreight.htb -> 192.168.10.10 -> /var/www/backup

- These domain can refer to same IP, but internally these are separated and distinguished using different folders.

#### `How to -->`

>[!Note]
>During the subdomain discovering activities, we have seen some subdomains having same IP addresses that can either be virtual hosts or in some cases, different servers under proxy.

##### `Manual Process (vHost Fuzzing) -->`

- Let's say we have found a webserver at 192.168.10.10.

`curl -s http://192.168.10.10` --> it shows a default website

- Let's make a `cURL` request sending a domain previously identified during the information gathering in the `HOST` header. We can do that like so:

`curl -s http://192.168.10.10 -H randomtarget.com` --> getting results indicates that its not hosting a single website and may hold some virtual hosts.

- So we enumerate with possible virtual hosts names by using **fuzzing** technique. using the vhosts list in [[information_gathering_tools]]

`cat ./vhosts | while read vhost;do echo "\n********\nFUZZING: ${vhost}\n********";curl -s -I http://192.168.10.10 -H "HOST: ${vhost}.randomtarget.com" | grep "Content-Length: ";done` --> based on the content length we can determine if there are any vhosts present

- If we can find any vhosts present then we found the subdomain of the target we can check it with curl.

`curl -s http://192.168.10.10 -H "Host: dev-admin.randomtarget.com"`

##### `Automated Process (vHost Fuzzing-->`

- The entire manual process can automated using tools like  [ffuf](https://github.com/ffuf/ffuf), we can speed up the process and filter based on parameters in the response.
- We can visit man page of this command for better understanding, basic ones are listed in [[information_gathering_tools]]



