
***
###### `Title :- User Account Control`
###### `Created on :- 2023-08-06 - 21:09`
###### `Created by:- Prem J`
***

- The large majority of home users are logged into their Windows systems as local administrators.
- Remember from the previous task [[Permissions and User Accounts]] that any user with administrator as the account type can make changes to the system.
- But there are risks in performing general tasks using an admin account. For instance, malware could affect the system and take control over the entire system.
- To protect the local user with such privileges, Microsoft introduced **User Account Control** (UAC). 
- This concept was first introduced with the short-lived [Windows Vista](https://en.wikipedia.org/wiki/Windows_Vista) and continued with versions of Windows that followed.

>[!Note]
>UAC (by default) doesn't apply for the built-in local administrator account

#### `How does it work -->`

- When a user with an account type of administrator logs into a system, the current session doesn't run with elevated permissions, instead When an operation requiring higher-level privileges needs to execute, the user will be prompted to confirm if they permit the operation to run.
- For instance, let's take an application that can only run on admin privileges.

![[Pasted image 20230806213042.png]]

- when a standard user downloads the same application, he might need admin level access inorder to run the application.

![[Pasted image 20230806213131.png]]

- That is what a standard user views upon downloading the application that requires admin level of access. Upon opening the application he will be prompted for admin credentials.

![[Pasted image 20230806213237.png]]

- This feature reduces the likelihood of malware successfully compromising your system. You can read more about [UAC](https://docs.microsoft.com/en-us/windows/security/identity-protection/user-account-control/how-user-account-control-works) here.

#### `Changing Settings -->`

+ The UAC settings can be changed or even turned off entirely (not recommended).

![[Pasted image 20230806232059.png]]

- Upon moving the slider we can view the options, it also requires admin privileges inorder to change UAC settings.