
***
###### `Title :- Volume shadow copy service`
###### `Created on :- 2023-08-07 - 07:05`
###### `Created by:- Prem J`
***

- the ***Volume Shadow Copy Service*** (VSS) coordinates the required actions to create a consistent shadow copy (also known as a snapshot or a point-in-time copy) of the data that is to be backed up.
- Volume Shadow Copies are stored on the System Volume Information folder on each drive that has protection enabled.

![[Pasted image 20230807070626.png]]

- If VSS is enabled (**System Protection** turned on), you can perform the following tasks from within **advanced system settings**. 	
	- **Create a restore point**
	- **Perform system restore**
	- **Configure restore settings**
	- **Delete restore points**

- From a security perspective, malware writers know of this Windows feature and write code in their malware to look for these files and delete them. 
- Doing so makes it impossible to recover from a ransomware attack unless you have an offline/off-site backup.

![[Pasted image 20230807070807.png]]
