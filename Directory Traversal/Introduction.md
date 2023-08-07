
***
###### `Title :- Introduction`
###### `Created on :- 2023-07-12 - 07:27`
###### `Created by:- Prem J`
***

- Directory traversal (also known as file path traversal) is a web security vulnerability that allows an attacker to read arbitrary files on the server that is running an application.
- This might include application code and data, credentials for back-end systems, and sensitive operating system files.
- In some cases we might gain the opportunity to write files into the system.

#### `Reading Arbitrary files through DT -->`

- Consider the following HTML

```html
<img src="/loadImage?filename=218.png"> -- /var/www/images/218.png
```

- There is a direct specification of the filename which pulls the picture, we can leverage this to read files from the server.
- The application implements no defenses against directory traversal attacks, so an attacker can request the following URL to retrieve an arbitrary file from the server's filesystem:

```url
https://insecure-website.com/loadImage?filename=../../../etc/passwd
```

- the above url causes the application to read from the `/var/www/images/../../../etc/passwd` file.

>[!Note]
>On Unix-based operating systems, this is a standard file containing details of the users that are registered on the server.
>On Windows, both `../` and `..\` are valid directory traversal sequences, and an equivalent attack to retrieve a standard operating system file would be:
>
> `https://insecure-web.com/loadImage?filename=..\..\..\windows\win.ini`



