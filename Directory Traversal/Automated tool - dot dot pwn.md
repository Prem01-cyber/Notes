
***
###### `Title :- Automated tool - dot dot pwn`
###### `Created on :- 2023-07-12 - 14:13`
###### `Created by:- Prem J`
***

#### `Manual Way -->`

- [Directory Traversal Report Practical](https://hackerone.com/reports/333306) for URL Encoding decrypt ( Burpsuite Decoder )

- General Get Request in the request `filename`

![[Screenshot from 2023-07-12 13-47-38.png]]

- We should shave got the results in the below request but there is an encoding problem so we dont the desired results

![[Screenshot from 2023-07-12 13-49-00.png]]

- And the problem gets solved

![[Screenshot from 2023-07-12 13-53-28.png]]

- This is the manual approach which is a bit hard and there are tools that automate the process for us.

### `Automated Way -->`

#### `Using tool on Juice shop -->`

- dot dot pwn is an automated directory traversal fuzzing tool. we can install it through apt using the command `sudo apt-get install dotdotpwn`
- Example scenario `sudo dotdotpwn -m http -h localhost:3000/#/login` the website  is the juiceshop hosted on the port 3000.
- As the tool runs it gives all the possibilities of the directory traversal.