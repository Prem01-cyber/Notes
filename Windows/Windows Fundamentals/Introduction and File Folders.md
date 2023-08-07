***
###### `Title :- Introduction`
###### `Created on :- 2023-08-06 - 00:55`
###### `Created by:- Prem J`
***

- The Windows operating system (OS) is a complex product with many system files, utilities, settings, features, etc.

>[!Note]
>This notes is entirely based on Tryhackme rooms, having said that the systems from tryhackme can also be accesed through Remote Desktop Protocol (RDP). Additional info can be found in [Remote Desktop](https://www.cyberark.com/resources/threat-research-blog/explain-like-i-m-5-remote-desktop-protocol-rdp).

- Windows 10 comes in 2 flavors, Home and Pro. You can read the difference between the Home and Pro [here](https://www.microsoft.com/en-us/windows/compare-windows-10-home-vs-pro).

>[!Note] 
>The Windows edition for the attached VM is Windows Server 2019 Standard, as seen in **System Information**.

#### `File System -->`

- The file system used in modern versions of Windows is the **New Technology File System** or simply [NTFS](https://docs.microsoft.com/en-us/windows-server/storage/file-server/ntfs-overview).
- Before NTFS, there was **FAT16/FAT32** (File Allocation Table) and **HPFS** (High Performance File System).
- You still see FAT partitions in use today. For example, you typically see FAT partitions in USB devices, MicroSD cards, etc. but traditionally not on personal Windows computers/laptops or Windows servers.

#### `NTFS -->`

- NTFS is known as a ***journaling file system***. In case of a failure, the file system can automatically repair the folders/files on disk using information stored in a log file. This function is not possible with FAT.
- NTFS addresses many of the limitations of the previous file systems; such as: 
	- Supports files larger than 4GB
	- Set specific permissions on folders and files
	- Folder and file compression
	- Encryption ([Encryption File System](https://docs.microsoft.com/en-us/windows/win32/fileio/file-encryption) or **EFS**)

- On NTFS volumes, you can set permissions that grant or deny access to files and folders. The permissions are:
	- **Full control**
	- **Modify**
	- **Read & Execute**
	- **List folder contents**
	- **Read**
	- **Write**

![[Pasted image 20230806204700.png]]

- we can view the file or folder permission through the properties of the respective file or folder
- Another feature of NTFS is **Alternate Data Streams** (**ADS**). It is a file attribute specific to windows NTFS
- Every file has at least one data stream (`$DATA`), and ADS allows files to contain more than one stream of data. Natively [Window Explorer](https://support.microsoft.com/en-us/windows/what-s-changed-in-file-explorer-ef370130-1cca-9dc5-e0df-2f7416fe1cb1) doesn't display ADS to the user. 
- There are 3rd party executables that can be used to view this data, but [Powershell](https://docs.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7.1) gives you the ability to view ADS for files.
- From a security perspective, malware writers have used ADS to hide data.
- Not all its uses are malicious. For example, when you download a file from the Internet, there are identifiers written to ADS to identify that the file was downloaded from the Internet.

>[!tip]
>To learn more about ADS, refer to the following link from MalwareBytes [here](https://blog.malwarebytes.com/101/2015/07/introduction-to-alternate-data-streams/).

#### `Windows Folders -->`

- The Windows folder (`C:\Windows`) is traditionally known as the folder which contains the Windows operating system.
- This folder doesn't actually need to reside in `C:\` but can be in other drive and technically can reside in a different folder.
- This is where environment variables, more specifically system environment variables, come into play. 
- Even though not discussed yet, the system  environment variable for the Windows directory is `%windir%`.

![[Pasted image 20230806205715.png]]

>[!Note]
>Per [Microsoft](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_environment_variables?view=powershell-7.1), "_Environment variables store information about the operating system environment. This information includes details such as the operating system path, the number of processors used by the operating system, and the location of temporary folders_".

#### `System32 -->`

- One of the many folders in `%windir%` i.e windows traditonal home directory is  **System32**.

![[Pasted image 20230806205927.png]]

- The System32 folder holds the important files that are critical for the operating system DLL's and other stuff.

>[!Danger]
>You should proceed with extreme caution when interacting with this folder. Accidentally deleting any files or folders within System32 can render the Windows OS inoperational. Read more about this action [here](https://www.howtogeek.com/346997/what-is-the-system32-directory-and-why-you-shouldnt-delete-it/).

