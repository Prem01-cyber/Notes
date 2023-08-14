
***
###### `Title :- Environment Enumeration`
###### `Created on :- 2023-08-14 - 22:04`
###### `Created by:- Prem J`
***

- Enumeration is the key to privilege escalation. Several helper scripts (such as [LinPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS) and [LinEnum](https://github.com/rebootuser/LinEnum) exist to assist with enumeration.
- But we do need to have an idea of manual enumeration i.e OS version, Kernel Version, Running services

#### `Gaining Situational Awareness -->`

- After exploiting a system and gaining reverse shell access we need to know some info regarding the system we have gained access to.

1. `whoami` - user we are running as
2. `id` - id of the user
3. `hostname` - hostname of the user
4. `ip addr show` - subnet we landed in
5. `sudo -l` - sudo privileges given to the user
6. `echo $PATH` - we can manipulate the order and leverage this to escalate privileges
7. `uname -a` - to find the OS details - `cat /proc/version`
8. `lscpu` - information regarding CPU
9. `cat /etc/shells` - logged in shells

- We should also check to see if any defenses are in place and we can enumerate any information about them. Some things to look for include:
	- [Exec Shield](https://en.wikipedia.org/wiki/Exec_Shield)
	- [iptables](https://linux.die.net/man/8/iptables)
	- [AppArmor](https://apparmor.net/)
	- [SELinux](https://www.redhat.com/en/topics/linux/what-is-selinux)
	- [Fail2ban](https://github.com/fail2ban/fail2ban)
	- [Snort](https://www.snort.org/faq/what-is-snort)
	- [Uncomplicated Firewall (ufw)](https://wiki.ubuntu.com/UncomplicatedFirewall)

- Often we will not have the privileges to enumerate the configurations of these protections but knowing what, if any, are in place, can help us not to waste time on certain tasks.

10. `lsblk` - drive information
11. `lpstat` - printers attached to the system to check for queued print jobs
12. `cat /etc/fstab` - for mounted drives
13. `route` - networks available via which interface - `netstat -rn`
14. `cat /etc/resolv.conf` - is essential if target is configured to used DNS internally
15. `arp -a` - other hosts that our system is communicating with
16. `/etc/passwd` - user information
17. `/etc/shadow` - hashed password information

>[!Note]
>With Linux, several different hash algorithms can be used to make the passwords unrecognizable. Identifying them from the first hash blocks can help us to use and work with them later if needed. Here is a list of the most used ones:
>
>Salted MD5 - `$1$...`
>SHA-256 - `$5$...`
>SHA-512 - `$6$`
>BCrypt - `$2a$`
>SCrypt - `$7$`
>Argon2 - `$argon21$`

>[!Tip]
>We'll also want to check which users have login shells. Once we see what shells are on the system, we can check each version for vulnerabilities. Because outdated versions, such as Bash version 4.1, are vulnerable to a `shellshock` exploit.

18. `/etc/group` - information regarding the groups

>[!TIp]
>The `/etc/group` file lists all of the groups on the system. We can then use the [getent](https://man7.org/linux/man-pages/man1/getent.1.html) command to list members of any interesting groups.
>`getent group sudo`

19. `ls -l /home` - home directory verification and checking the files in it

- Finally, we can search for any "low hanging fruit" such as config files, and other files that may contain sensitive information.
- These configuration files might include valuable information.
- If we've gathered any passwords we should try them at this time for all users present on the system. Password re-use is common so we might get lucky!

20. `df -h` - mounted file system listing in human readable format
21. `cat /etc/fstab | grep -v "#" | column -t` - listing unmounted files

- When a file is unmounted, it indicates that it's a possible threat to the host. Hence, if we are able to mount into the file system, we might be lucky enough to gain root level privileges.

22. `find / -type f -name ".\*" -exec ls -l {} \\; 2\>/dev/null` - hidden file search
23. `find / -type d -name ".\*" -ls 2\>/dev/null` - hidden dir search

- In addition, three default folders are intended for temporary files. These folders are visible to all users and can be read
- In addition, temporary logs or script output can be found there
- Both `/tmp` and `/var/tmp` are used to store data temporarily, but data retention of `/var/tmp - 30d` is longer than `/tmp - restart`

- All of this done and analyzed by [LinPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS) automatically. It is a proffered option in real world scenarios