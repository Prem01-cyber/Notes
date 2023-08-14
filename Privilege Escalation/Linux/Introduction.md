
***
###### `Title :- Introduction`
###### `Created on :- 2023-08-14 - 21:50`
###### `Created by:- Prem J`
***

- The root account on Linux Systems provide full admin level access to the OS.
- In the process of assessment we may get access to Low level account, we will be pivoting through the systems to reach the root account.

#### `Enumeration -->`

- It's the first steps that are need to be taken after compromising a target.
- There are automated ways and manual ways to conquer this some of them include scripts like [LinuxEnum](https://github.com/rebootuser/LinEnum) which assist with the process.
- It is important to check for some details when you have initial shell access. Because based on the information we gather we may find **Some Publicly available exploits that are useful for pivoting**.

- When it comes to privilege escalation enumeration plays a huge role in having our privileges increased

1.  OS version - `cat /etc/lsb-release`
2. Kernel version - `cat uname -a`
3. Running Services - `ps aux`
4. Process ran by root - `ps au | grep root`
5. Installed Packages and version - `-v option with the package`
6. Logged in users - `ps au`
7. home directory - `ls /home`
8. User Specific home directory - `ls /home/jaggy`
9. ssh contents - `ls -l ~/.ssh`
10. bash history - `history`
11. sudo privileges - `sudo -l` or `sudo -ll`
12. configuration files - `.confg or .conf`
13. Readable shadow file - `john to crack password with passwd file`
14. User file - `/etc/passwd`
15. Cron jobs - `/etc/cron*`
16. drive info - `lsblk`
17. search for `suid` and `uid` bit set files 
18. Writeable files

- [[find]] command can be very useful in some scenarios. 

[^1]: Kernel exploits can cause instability to the system, we need be sure to understand the exploit before using it.
[^2]: A search for SSH Keys can be done get a hold of other systems by gaining a stable and fully interactive session or even gain entry into the Active Directory.
[^3]: To list the possible commands that can be run using sudo we can use sudo -l (list sudo processes)