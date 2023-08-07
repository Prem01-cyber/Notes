
***
###### `Title :- IMAP`
###### `Created on :- 2023-05-21 - 14:26`
###### `Created by:- Prem J`
***
#### `Brief -->`

- Text based (ASCII) network Protocol for online management of emails on a remote server.

#### `Used For -->`

- Online management of emails directly on server and supports folder structure.
- Allows synchronization of a local email client with the mailbox on the server, providing a kind of network file system for emails, allowing problem-free synchronization across several independent clients
- Several users can access the email server simultaneously.
- There has to be an active connection to manage emails, but some clients offer offline mode with local copy of the mailbox. The client synchronizes all offline local changes when a connection is reestablished.

#### `How does it work -->`

- Client establishes connection to the server via port **143**. it uses text based ascii cmds. Several commands can be sent without waiting for any confirmations from the sever. Later the confirmations are assigned i.e we assign the request with number 1 and we get the reply to it later.

		1 LOGIN robin robin --> 1 OK 
		2 SELECT INBOX --> 2 OK

- Access to the desired mailbox is only possible after succesful authentication.
- SMTP[^1] is used for sending mails, by copying all the sent emails into a IMAP folder all the clients have access to the mails remotely.

#### `Advantages -->`

- We can create personal folder and folder structures in the mail box to organise the emails.

#### `Alternatives -->`



#### `Configuration Files -->`

- IMAP and POP3[^2] can have large configuration options. to deep dive into each component in more detail. If you wish to examine these protocol configurations deeper, we recommend creating a VM locally and install the two packages **dovecot-imapd**, and **dovecot-pop3d** using apt.
- In the documentation of Dovecot, we can find the individual [core settings](https://doc.dovecot.org/settings/core/) and [service configuration](https://doc.dovecot.org/configuration_manual/service_configuration/) options.

#### `Commands -->`

- This is the best resource for [imap commands](https://www.atmail.com/blog/imap-101-manual-imap-sessions/), commands are not case-sesitive.

| POP3 Commands | Description | 
| :-- | :-- |
| USER username | Identifies the user |
| PASS password | authenticating the user |
| STAT | requests the number of saved emails from the server |
| LIST | requests form the server the number and size of all emails |
| RETR id | requests the server to deliver the requested email by id |
| DELE id | requests the server to delete the requested email by id |
| CAPA | requests the server to display the server capabilities |
| RSET | requests the server to reset the transmitted information |
| QUIT | closes the connection with the server |

#### `Vulnerabilities -->`

1. IMAP is unerncrypted and transmits emails, passwords, usernames in plain text.
2. Misconfigurations, Many companies use Google -> Gmail, Microsoft -> Outlook, and many other. But some companies use their mail servers for many reasons mainly privacy. Many configuration errors takes place by the admins, which in the worst case scenario will allow us to read all the mails, which may contain confidential information.
	- auth_debug -> enables all authentication debugging logging.
	- auth_debug_passwords -> adjusts log verbosity leadis to obtain submitted passwords
	- auth_verbose -> logs failed log attemps and their reasons
	- auth_verbose_passwords -> passwords used for auth are logged and are also truncated
	- auth_anonymous_username -> username to be used while ANONYMOUS SASL[^3] mechanism.

#### `Remedies -->`

1. As the transmissions are unencrypted many servers use **TLS/SSL** over IMAP sessions to make them encrypted. Depending on the method used IMAP uses **143** or **993**.

#### `Footprinting Walkthrough -->`

- We start by footprinting the service using **[[Nmap]]**[^4] , By the default ports **110**, **143**, **993**, **995** Which are used by IMAP and POP3.

> 	sudo nmap 10.129.14.128 -sV -p110,143,993,995 -sC


- Based on the above result we can find the terms like **commonName**, **organisationName**, and respective locations, if the target has an **SSL Certificate**. We can also find the commands that can be used on the respective server. we can also attain the banner of the service through the above enumeration
- If we successfully **figure out the access credentials** for one of the employees, an attacker could log in to the mail server and read or even send the individual messages.
- Now we need to figure out the version of TLS that is used with target, for which we will we using [[curl]] with verbose option and figured out creds.

> 	curl -k 'imaps://10.129.14.128' --user cry0l1t3:1234 -v

- Now to interact with IMAP or POP3 over SSL we can use **openssl** or **ncat**.

>	openssl s_client -connect 10.129.14.128:pop3s -> preferred way
>	openssl s_client -connect 10.129.14.128:imaps -> preferred way
>	curl -k 'imaps://<FQDN/IP>' --user username:password 
>	telnet 10.129.14.129 110 alt 995
>	nc -n 10.129.14.129 110

#### `Summary -->`

- Once we have successfully initiated a connection and logged in to the target mail server, we can use the above commands to work with and navigate the server. We want to point out that the configuration of our own mail server, the research for it, and the experiments we can do together with other community members will give us the know-how to understand the communication taking place and what configuration options are responsible for this.


[^1]: SMTP
[^2]: POP3, on the other hand, does not have the same functionality as IMAP, and it only provides listing, retrieving, and deleting emails as functions at the email server
[^3]: ANONMYOUS SASL
[^4]: incase if we want to use scripts,  one of the scripts associated with imap is **imap-ntlm-info**