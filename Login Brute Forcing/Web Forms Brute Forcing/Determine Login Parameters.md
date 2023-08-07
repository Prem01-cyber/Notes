
***
###### `Title :- Determine Login Parameters`
###### `Created on :- 2023-06-26 - 06:19`
###### `Created by:- Prem J`
***

- Upon intercepting the requests we can find the parameters, this can be achieved through various ways like using browser and [[burpsuite]]

![[Pasted image 20230626062700.jpg]]

![[Pasted image 20230626062719.jpg]]

```bash
"/login.php:username=^USER^&password=^PASS^:F=<form name='login'"
```