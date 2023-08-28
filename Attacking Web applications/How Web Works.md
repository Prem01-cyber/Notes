
***
###### `Title :- How Web Works`
###### `Created on :- 2023-08-19 - 03:14`
###### `Created by:- Prem J`
***

### `Brief -->`

- when you request a website, your computer needs to know the server's IP address it needs to talk to; for this, it uses ***DNS***
- Your computer then ***talks*** to the ***web server*** using a special set of commands called the ***HTTP protocol***
- the webserver then returns ***HTML, JavaScript, CSS, Images, etc.,*** which your ***browser*** then uses to correctly format and ***display*** the website to you.

![[Pasted image 20230819031652.png]]

### `Other Components -->`

#### `1. Load balancers `

![[829e340231cd8aa9f5ed2fa5c464ea80.svg]]

- When there is an application that needs to have high availability, one webserver might no be enough to do the job.
- Load balancers provide ***two main features***, ensuring high traffic websites can handle the load and providing a fail-over if one of the servers becomes unresponsive.

- When you request a website with a load balancer, the load balancer will receive your request first and then forward it to one of the multiple servers behind it
- The load balancer uses different algorithms to help it decide which server is best to deal with the request
- couple of them are ***round-robin***, which sends it to each server in turn, or ***weighed*** which checks how many requests a server is currently dealing with and sends it to the least busy server.