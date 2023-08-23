
- Shodan is an excellent tool for those who are new to the world of bug bounty hunting or those who are timid.
- When they read a program and it says that you should not scan a network and you don't really want to run in map and you're too afraid to actually scan the network.
- You can go check out Shodan because SHODAN scans the network automatically and then stores all the information in its database.
- So all we have to do is know what queries to run to pull the information from Shodan and read their information from when they have crawled the network or the web browser previously.
- And then we have access to all of that information without having to scan the actual target.
- And you don't have to worry about breaking any program rules.
- So the way Shodan works is it actually goes out and crawls every single device that is connected to the Internet, whether it be your thermostat, your refrigerator, or your Web security cameras.
- And it'll see if there's any vulnerabilities.
- It'll store the information such as the software that is currently running on it.
- And if it has any vulnerabilities, it is now open to the Web.
- And anyone can access your information, your thermostat, your refrigerator, your webcam, or even your printers.

#### `Terminal Version -->`

- Initialize shodan in the dir using the API Key

`shodan init <API KEY>`
`shodan info` --> Gives account status like query and scan credits
`shodan count wordpress` --> servers that run wordpress
`shodan count wordpress 1.4.7` --> More specific results
`shodan download <file-name> wordpress 1.4.7` --> save the vulnerable server data to file

- What nmap and shodan does are similar but shodan does the heavy lifting for us.

>[!Note]
>`host yahoo.com`
>This would give us the IP addresses that yahoo.com is hosted on, we can use `ping` to verify that. This can be alternatively found through burp proxy.

`shodan host <IP>` --> Its gonna give us some information, ports and stuff

- And so the difference here between what we're able to find between Google and Shodan is Google goes out and crawls the pages to see what is reachable, but SHODAN crawls all of the Internet connected devices.
- And so Shodan is a great way to scan a network without actually scanning a network, because Shodan already has stored all of the information that you're going to be looking for, and you just have to go out and find it.

- If you're looking for open ports, I personally would rather just run in map with the ports that I want to find, such as port 22, port 80 443 144 333 89 or whatever other ports that you might come across. But I would rather run in map with specific ports.

`shodan scan submit <IP/IPs/List>` --> scanning using shodan