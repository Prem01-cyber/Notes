
***
###### `Title :- Combination Attack`
###### `Created on :- 2023-06-27 - 22:01`
###### `Created by:- Prem J`
***

- Takes two wordlists as input and creates combinations from them.

#### `Combining Wordlists -->`

- Wordlists can be combined directly and then used or hashcat does it for us
- Below is done by `awk`, which combine the given wordlists, and generates a new wordlist.

```shell-session
awk '(NR==FNR) { a[NR]=$0 } (NR != FNR) { for (i in a) { print $0 a[i] } }' file2 file1
```

- Alternatively we can give the two wordlists to the `Hashcat` which will do the whole process automatically.

```shell-session
hashcat -a 1 --stdout file1 file2  #does not perform password cracking
```

- the option `-a` is 1, as it says that the attack mode is `Combination Attack`. ([[Hashcat Overview]])

#### `Hashcat Syntax -->`

```shell-session
htb[/htb]$ hashcat -a 1 -m 0 combination_md5 wordlist1 wordlist2
```

- Combination attacks are another powerful tool to keep in our arsenal. As demonstrated above, merely combining two words does not necessarily make a password stronger.
