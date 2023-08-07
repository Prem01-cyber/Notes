
***
###### `Title :- Windows Security`
###### `Created on :- 2023-08-07 - 01:37`
###### `Created by:- Prem J`
***

- Is just the control panel for firewall and protection, which include virus protection, firewall and network protection, app and browser control and Device security.

![[Pasted image 20230807014140.png]]

- red indicates warning, green indicates we are good to go and finally yellow indicates there are some recommendations but you are fine.

#### `Virus and threat Protection -->`

- Virus & threat protection is divided into two parts:  
	- **Current threats**
	- **Virus & threat protection settings**

![[Pasted image 20230807014339.png]]

- we can scan for threats and there are several options available to perform scanning. 
- In the run we can also view our previous threats which were found in our systems

- There are several settings available when it comes to virus protection
	- **Real-time protection** - Locates and stops malware from installing or running on your device.
	- **Cloud-delivered protection** - Provides increased and faster protection with access to the latest protection data in the cloud.
	- **Automatic sample submission** - Send sample files to Microsoft to help protect you and others from potential threats. 
	- **Controlled folder access** - Protect files, folders, and memory areas on your device from unauthorized changes by unfriendly applications.
	- **Exclusions** - Windows Defender Antivirus won't scan items that you've excluded.
	- **Notifications** - Windows Defender Antivirus will send notifications with critical information about the health and security of your device.

- having said that there are also protection updates which can be manually checked by performing a check for windows defender antivirus definitions
- ***Ransomware protection*** requires this feature to be enabled, which in turn requires Real-time protection to be enabled.

>[!Note] 
>Real-time protection should definitely be enabled in your personal Windows devices unless you have a 3rd party product that provides the same protection. Ensure it's always up-to-date and enabled.  

>[!Tip] 
>You can perform on-demand scans on any file/folder by right-clicking the item and selecting 'Scan with Microsoft Defender'.

![[Pasted image 20230807014818.png]]

#### `Firewall and Network Protection -->`

- Traffic flows into and out of devices via what we call ports. 
- A firewall is what controls what is - and more importantly isn't - allowed to pass through those ports. 
- You can think of it like a security guard standing at the door, checking the ID of everything that tries to enter or exit

![[Pasted image 20230807015028.png]]

- **Domain** - The domain profile applies to networks where the host system can authenticate to a domain controller.
- **Private** - The private profile is a user-assigned profile and is used to designate private or home networks.
- **Public** - The default profile is the public profile, used to designate public networks such as Wi-Fi hotspots at coffee shops, airports, and other locations.

![[Pasted image 20230807015137.png]]

>[!Danger]
>Unless or until you are confident do not interfere with the settings of the networks

- We can also allow specific apps through firewall by altering windows defender firewall settings in control panel

![[Pasted image 20230807015302.png]]

- You can view what the current settings for any firewall profile are. In the above image, several apps have access in the Private and/or Public firewall profile. Some of the apps will provide additional information if it's available via the `Details` button.

+ Configuring the **Windows Defender Firewall** is for advanced Windows users. Refer to the following Microsoft documentation on best practices [here](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-firewall/best-practices-configuring).
+ Command to open the Windows Defender Firewall is `WF.msc`.

![[Pasted image 20230807015339.png]]

#### `App and Browser Control -->`

- ***Microsoft Defender SmartScreen*** protects against phishing or malware websites and applications, and the downloading of potentially malicious files

![[Pasted image 20230807015524.png]]

- SmartScreen helps by identifying unrecognised apps and file from the web
- Exploit protection is built in to windows server 2019 to protect your device against attacks.

![[Pasted image 20230807015619.png]]

>[!Danger]
>Unless you are 100% confident in what you are doing, it is recommended that you leave the default settings.

#### `Device Security -->`

![[Pasted image 20230807015734.png]]

- Core isolation prevents attacks from inserting malicious code into high-security processes (**Memory Integrity**)

![[Pasted image 20230807015831.png]]

![[Pasted image 20230807015839.png]]

![[Pasted image 20230807015849.png]]

- `TPM` - Trusted Platform Module technology is designed to provide hardware-based, security-related functions. 
- A TPM chip is a secure crypto-processor that is designed to carry out cryptographic operations. 
- The chip includes multiple physical security mechanisms to make it tamper-resistant, and malicious software is unable to tamper with the security functions of the TPM