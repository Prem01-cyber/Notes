- The root account on Linux Systems provide full admin level access to the OS.
- In the process of assessment we may get access to Low level account, we will be pivoting through the systems to reach the root account.

#### `Enumeration -->`

- It's the first steps that are need to be taken after compromising a target.
- There are automated ways and manual ways to conquer this some of them include scripts like [LinuxEnum](https://github.com/rebootuser/LinEnum) which assist with the process.
- It is important to check for some details when you have initial shell access. Because based on the information we gather we may find **Some Publicly available exploits that are useful for pivoting**.

| Property | Description |
| -- | -- |
|OS Version  | **cat /etc/issue**|
|System Info | uname -a |
|hostname and Environmental variables | hostname, env |
|Kernel Version[^1] | **cat /proc/version**|
|Running Services | Service misconfigurations can cause a great deal of damage (**ps aux**)|
|Installed Packages and Versions | Outdated packages are dangerous to the system.|
|Logged in Users | Knowing which users are logged and their actions (**w**)|
|User home directories | a search for hidden content (**SSH Keys[^2]**, .bash_history, .e.t.c)|
|SSH Directory contents | ls ~/.ssh|
|Bash History | .bash_history file or **history** command |
|Sudo Privileges | sudo cmds that can be run without password (/etc/sudoers)[^3]|
|.conf files | a search for .conf files using find|
|Readable shadow file | /etc/shadow - usually passwords are hashed and stored|
|Password hash in passwd file | These can be subjected to password cracking attack|
|Cron Jobs | improper ones are dangerous (/etc/crontab)|
|Unmounted File Systems and Additional Drives | we may find sensitive files|
|SETUID and SETGID | improper configurations can lead to root level access.|
|Writable Directories | You may discover a writable directory where a cron job places files, which provides an idea of how often the cron job runs and could be used to elevate privileges if the script that the cron job runs is also writeable.  |
|Writable Files | While altering configuration files can be extremely destructive, there may be instances where a minor modification can open up further access.|

- [[find]] command can be very useful in some scenarios. 

[^1]: Kernel exploits can cause instability to the system, we need be sure to understand the exploit before using it.
[^2]: A search for SSH Keys can be done get a hold of other systems by gaining a stable and fully interactive session or even gain entry into the Active Directory.
[^3]: To list the possible commands that can be run using sudo we can use sudo -l (list sudo processes)