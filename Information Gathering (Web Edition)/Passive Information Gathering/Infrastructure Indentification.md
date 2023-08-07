
***
###### `Title :- Infrastructure Indentification`
###### `Created on :- 2023-05-27 - 20:08`
###### `Created by:- Prem J`
***
#### `Brief -->`

- We use tools to find out about the servers without interacting with them.

#### `How to -->`

- We use [Netcraft](https://sitereport.netcraft.com) to find the information about the target infrastructure, There are some interesting details that we can find in the report.

**Background** --> General information about the domain, date when it was first seen by netcraft crawlers[^1] .
**Network** --> Information about netblock owner, hosting company, nameservers .e.t.c.
**Hosting History** --> **Latest IP's used**, webserver, and target OS details.

- The [Internet Archive](https://en.wikipedia.org/wiki/Internet_Archive) is an American digital library that provides free public access to digitalized materials, including websites, collected automatically via its web crawlers.
- [Wayback Machine](http://web.archive.org/) is used to find the old versions that have interesting comments like that version of the website is having a particular vulnerability. By which we can derive to certain conclusions.

- We can also use the tool [waybackurls](https://github.com/tomnomnom/waybackurls) to inspect URLs saved by Wayback Machine and look for specific keywords

>[!tip]
>We need **Go** to setup the waybackurls tool on our machine
>
>`go install github.com/tomnomnom/waybackurls@latest`

- To get a list of crawled URLs from a domain with the date it was obtained, we can add the `-dates` switch to our command as follows:

>	`waybackurls -dates https://facebook.com \> waybackurls.txt`

