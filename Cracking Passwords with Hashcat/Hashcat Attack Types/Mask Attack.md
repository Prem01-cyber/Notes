
***
###### `Title :- Mask Attack`
###### `Created on :- 2023-06-27 - 22:22`
###### `Created by:- Prem J`
***

- Mask attacks are used to generate words matching a specific pattern.
- This type of attack is particularly useful when the password length or format is known. A mask can be created using static characters, ranges of characters (e.g. `[a-z]` or `[A-Z0-9]`), or placeholders.

|**Placeholder**|**Meaning**|
|---|---|
|?l|lower-case ASCII letters (a-z)|
|?u|upper-case ASCII letters (A-Z)|
|?d|digits (0-9)|
|?h|0123456789abcdef|
|?H|0123456789ABCDEF||
|?a|?l?u?d?s|
|?b|0x00 - 0xff|
|?s|special characters («space»!"#$%&'()*+,-./:;<=>?@[]^_`{|

- The above placeholders can be combined with options "`-1`" to "`-4`" which can be used for custom placeholders. 
- See the _Custom charsets_ section [here](https://hashcat.net/wiki/doku.php?id=mask_attack) for a detailed breakdown of each of these four command-line parameters that can be used to configure four custom charsets.

- Consider the company Inlane Freight, which this time has passwords with the scheme "`ILFREIGHT<userid><year>`," where userid is 5 characters long. 
- The mask "`ILFREIGHT?l?l?l?l?l20[0-1]?d`" can be used to crack passwords with the specified pattern, where "`?l`" is a letter and "`20[0-1]?d`" will include all years from 2000 to 2019.

````shell-session
hashcat -a 3 -m 0 md5_mask_example_hash -1 01 'ILFREIGHT?l?l?l?l?l20?1?d'
````

- The "`-1`" option was used to specify a placeholder with just 0 and 1. 
- `Hashcat` could crack the hash in 43 seconds on CPU power. The "`--increment`" flag can be used to increment the mask length automatically, with a length limit that can be supplied using the "`--increment-max`" flag.