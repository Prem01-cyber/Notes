***
###### `Title :- Infrastructure Identification`
###### `Created on :- 2023-05-27 - 22:42`
###### `Created by:- Prem J`
***
#### `Brief -->`

- A web application's infrastructure is what keeps it running and allows it to function. Web servers are directly involved in any web application's operation. Some of the most popular are Apache, Nginx, and Microsoft IIS, among others

## `Web Servers -->`

- We need to discover **as much information as possible** from the web server to understand it's functionality. as we are performing active scanning we might get blocked, which would affect our testing process.

>[!tip]
>To load a vhost linked to a IP address we need to the vHost inside of /etc/hosts file so that our browser knows the path to the target
>![[Screenshot from 2023-05-29 10-42-55.png]] 

#### `How to -->`

- If we can discover the webserver behind the target, it can give us a good idea of what target Operating System is running on the back end server.

- IIS 6.0: Windows Server 2003
- IIS 7.0-8.5: Windows Server 2008 / Windows Server 2008R2
- IIS 10.0 (v1607-v1709): Windows Server 2016
- IIS 10.0 (v1809-): Windows Server 2019

>[!error]
>this is usually correct when dealing with Windows, but cant be sure with Linux or BSD 
>based distributions

- the first thing we need to identify the **webserver version** by looking at the response headers using [[curl]]

>	`curl -I "http://${TARGET}"`

- There are also other characteristics to take into account while fingerprinting web servers in the response headers

1. **X-Powered-By header**: This header can tell us what the web app is using. We can see values like PHP, ASP.NET, JSP, etc.
2. **Cookies**: Cookies are another attractive value to look at as each technology by default has its cookies.

- Other available tools analyse common web server characteristics by probing them and comparing their responses with a database of signatures to guess information like web server version, installed modules, and enabled services. some of the tools are mentioned in [[information_gathering_tools]]. 

