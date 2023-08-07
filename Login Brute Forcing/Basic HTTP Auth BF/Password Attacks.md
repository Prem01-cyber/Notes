
***
###### `Title :- Password Attacks`
###### `Created on :- 2023-06-24 - 07:32`
###### `Created by:- Prem J`
***

- The HTTP specification provides two parallel authentication mechanisms:

1. `Basic HTTP AUTH` is used to authenticate the user to the HTTP server.    
2. `Proxy Server Authentication` is used to authenticate the user to an intermediate proxy server.

- These two work in a similar way by which they use the same **requests**, **response headers** and **Status codes**.

#### `Basic HTTP Auth -->`

- Any host that uses Basic HTTP Auth (a user ID and password) must follow [Basic HTTP AUTH](https://tools.ietf.org/html/rfc7617) scheme.
- The client sends a request without authentication information with its first request. The server's response contains the `WWW-Authenticate` header field, which requests the client to provide the credentials.
- This header field also defines details of how the authentication has to take place. The client is asked to submit the authentication information. In its response, the server transmits the so-called realm, a character string that tells the client who is requesting the data.
- The client uses the Base64 method for **encoding the identifier and password**. This encoded character string is transmitted to the server in the Authorization header field.

#### `Password Attacks -->`

- As we don't have user ID and password, nor we have the ports available, and no service information about the webserver to use or attack.
- We assert to utilize password brute-forcing, There are several types of password attacks

	1. Dictionary attack
	2. Brute Force
	3. Traffic Interception
	4. MITM
	5. Key Logging
	6. Social Engineering

#### `Brute Force Attack -->`

- literally means brute forcing with all the character combinations, It does not depend on any wordlist.
- The time consumed by this attack is too much as there are millions of possible permutations as the length of the password increases, making it a difficult task.
- All of this shows that relying completely on brute force attacks is not ideal, and this is especially true for brute-forcing attacks that take place over the network, like in `hydra`.

#### `Dictionery Attack -->`

- A `Dictionary Attack` tries to guess passwords with the help of lists. the goal here is to use a list of known password to find the unknown password.
- It has a better advantage than Brute forcing as dictionary attack should take less time when compared to brute forcing.

#### `Methods of Brute Force attacks -->`

- There are several methods used for brute forcing attacks

1. `Online Brute Force Attack` - Attacking a live application over the network, like HTTP, HTTPs, SSH, FTP, and others
2. `Offline Brute Force Attack ` - Also known as Offline Password Cracking, where you attempt to crack a hash of an encrypted password.
3. `Reverse Brute Force Attack` - Also known as username brute-forcing, where you try a single common password with a list of usernames on a certain service.
4. `Hybrid Brute Force Attack` - Attacking a user by creating a customized password wordlist, built using known intelligence about the user or the service