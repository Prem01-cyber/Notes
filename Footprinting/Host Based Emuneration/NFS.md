
***
###### `Title :- NFS`
###### `Created on :- 2023-08-05 - 15:43`
###### `Created by:- Prem J`
***

#### `Introduction -->`

- NFS stands for "Network File System" and allows a system to ***share directories and files with others over a network***. 
- By using NFS, users and programs can access files on remote systems almost as if they were local files. 
- It does this by mounting all, or a portion of a file system on a server. The portion of the file system that is mounted can be accessed by clients with whatever privileges are assigned to each file.

#### `How does it work -->`

- First, the client will request to mount a directory from a remote host on a local directory just the same way it can mount a physical device
- The mount service will then act to connect to the relevant mount daemon using ***RPC***.
- The server checks if the user has permission to mount whatever directory has been requested. 
- It will then return a file handle which uniquely identifies each file and directory that is on the server.
- If someone wants to access a file using NFS, an RPC call is placed to ***NFSD*** (the NFS daemon) on the server. This call takes parameters such as:
	-  The file handle -- NFS represents file and directories by using this
	-  The name of the file to be accessed
	-  The user's, user ID
	-  The user's group ID

- These are used in determining ***access rights to the specified file***. This is what controls user permissions, I.E read and write of files.

#### `What runs NFS -->`

- using NFS Protocol we can connect over multiple platforms effortlessly. A computer running windows can act as the NFS file server for other non-windows clients computers and vice versa

#### `Enumeration -->`

- `nfs-common` is the tool used to interact with any NFS share from your local machine
- It can be installed through `sudo apt install nfs-common`
- It is important to have this package installed on any machine that uses NFS, either as client or server.
- It includes programs such as: **lockd**, **statd**, **showmount**, **nfsstat,** **gssd**, **idmapd** and **mount.nfs**
- Primarily, we are concerned with ***showmount*** and ***mount.nfs*** as these are going to be most useful to us when it comes to extracting information from the NFS share

- we can use [[nmap]] to conduct a port scan over the target. `nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount 10.10.11.56` - nmap scan for rpcbind port

- Your client’s system needs a directory where all the content shared by the host server in the export folder can be accessed.
- The folder can be anywhere on the system, there is no restrictions to it. The target NFS share can be mounted using mount command `sudo mount -t nfs IP:share /tmp/mount/ -nolock`

>[!Note]
>By default, on NFS shares- ***Root Squashing*** is enabled, and prevents anyone connecting to the NFS share from having root access to the NFS volume. Remote root users are assigned a user ***nfsnobody*** when connected, which has the least local privileges, not what we want. However, if this is turned off, it can allow the creation of ***SUID*** bit files, allowing a remote user root access to the connected system.

>[!Note]
>**SUID** - So, what are files with the SUID bit set? Essentially, this means that the file or files can be run with the permissions of the file(s) owner/group. In this case, as the super-user. We can leverage this to get a shell with these privileges!

#### `How to exploit -->`

- it's fairly easy to understand. We're able to upload files to the NFS share, and control the permissions of these files. 
- We can set the permissions of whatever we upload, in this case a ***bash shell executable***. 
- We can then log in through SSH, as we did in the previous task- and execute this executable to gain a root shell!

- Due to compatibility reasons, we'll use a standard Ubuntu Server 18.04 bash executable, the same as the server's- as we know from our nmap scan. 
- You can download it [here](https://github.com/TheRealPoloMints/Blog/blob/master/Security%20Challenge%20Walkthroughs/Networks%202/bash). 
- If you want to download it via the command line, be careful not to download the github page instead of the raw script. 
- You can use `wget https://github.com/polo-sec/writing/raw/master/Security%20Challenge%20Walkthroughs/Networks%202/bash`

  NFS Access ->

        Gain Low Privilege Shell ->

            Upload Bash Executable to the NFS share ->

                Set SUID Permissions Through NFS Due To Misconfigured Root Squash ->

                    Login through SSH ->

                        Execute SUID Bit Bash Executable ->

                            ROOT ACCESS

#### `Exploitation -->`

![[Screenshot from 2023-08-05 17-18-50.png]]

>[!tip]
>Now, the moment of truth. Lets run it with "_./bash -p_". The -p persists the permissions, so that it can run as root with SUID- as otherwise bash will sometimes drop the permissions.

