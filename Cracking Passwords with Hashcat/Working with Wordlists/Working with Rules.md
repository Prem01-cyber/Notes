
***
###### `Title :- Working with Rules`
###### `Created on :- 2023-06-28 - 07:43`
###### `Created by:- Prem J`
***

- The rule-based attack is the most advanced and complex password cracking mode. 
- Rules help perform various operations on the input wordlist, such as prefixing, suffixing, toggling case, cutting, reversing, and much more
- A rule can be created using functions, which take a word as input and output it's modified version. 
- The following table describes some functions which are compatible with JtR as well as [[Hashcat]].

|**Function**|**Description**|**Input**|**Output**|
|---|---|---|---|
|l|Convert all letters to lowercase|InlaneFreight2020|inlanefreight2020|
|u|Convert all letters to uppercase|InlaneFreight2020|INLANEFREIGHT2020|
|c / C|capitalize / lowercase first letter and invert the rest|inlaneFreight2020 / Inlanefreight2020|Inlanefreight2020 / iNLANEFREIGHT2020|
|t / TN|Toggle case : whole word / at position N|InlaneFreight2020|iNLANEfREIGHT2020|
|d / q / zN / ZN|Duplicate word / all characters / first character / last character|InlaneFreight2020|InlaneFreight2020InlaneFreight2020 / IInnllaanneeFFrreeiigghhtt22002200 / IInlaneFreight2020 / InlaneFreight20200|
|{ / }|Rotate word left / right|InlaneFreight2020|nlaneFreight2020I / 0InlaneFreight202|
|^X / $X|Prepend / Append character X|InlaneFreight2020 (^! / $! )|!InlaneFreight2020 / InlaneFreight2020!|
|r|Reverse|InlaneFreight2020|0202thgierFenalnI|

- A complete list of functions can be found [here](https://hashcat.net/wiki/doku.php?id=rule_based_attack#implemented_compatible_functions). Sometimes, the input wordlists contain words that don't match our target specifications
- Words of length less than N can be rejected with `>N`, while words greater than N can be rejected with `<N`. A list of rejection rules can be found [here](https://hashcat.net/wiki/doku.php?id=rule_based_attack#rules_used_to_reject_plains).

>[!Note]
>Reject rules only work either with `hashcat-legacy`, or when using `-j` or `-k` with `Hashcat`. They will not work as regular rules (in a rule file) with `Hashcat`.


#### `Examples -->`

- Usual user behaviour suggests that they tend to replace letters with similar numbers, like "`o`" can be replaced with "`0`" or "`i`" can be replaced with "`1`".
- This is commonly known as `L33tspeak` and is very efficient. Corporate passwords are often prepended or appended by a year.
- The first letter word is capitalized with the `c` function. Then rule uses the substitute function `s` to replace `o` with `0`, `i` with `1`, `e` with `3` and a with `@`. 
- At the end, the year `2019` is appended to it. Copy the rule to a file so that we can debug it.

```shell-session
echo 'c so0 si1 se3 ss5 sa@ $2 $0 $1 $9' > rule.txt
echo 'password_ilfreight' > test.txt
```

- Rules can be debugged using the "`-r`" flag to specify the rule, followed by the wordlist.
- Upon debugging the rules we get the password

```shell-session
htb[/htb]$ hashcat -r rule.txt test.txt --stdout

P@55w0rd_1lfr31ght2019
```

- As expected, the first letter was capitalized, and the letters were replaced with numbers. Let's consider the password "`St@r5h1p2019`". 
- We can create the `SHA1` hash of this password via the command line

```shell-session
htb[/htb]$ echo -n 'St@r5h1p2019' | sha1sum | awk '{print $1}' | tee hash
```

- We can then use the custom rule created above and the `rockyou.txt` dictionary file to crack the hash using `Hashcat`.

```shell-session
htb[/htb]$ hashcat -a 0 -m 100 hash /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt -r rule.txt

hashcat (v6.1.1) starting...
<SNIP>

08004e35561328e357e34d07c53c7e4f41944e28:St@r5h1p2019
                                                 
Session..........: hashcat
Status...........: Cracked
Hash.Name........: SHA1
Hash.Target......: 08004e35561328e357e34d07c53c7e4f41944e28
Time.Started.....: Fri Aug 28 22:17:13 2020, (3 secs)
Time.Estimated...: Fri Aug 28 22:17:16 2020, (0 secs)
Guess.Base.......: File (/opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt)
Guess.Mod........: Rules (rule.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:  3519.2 kH/s (0.39ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests
Progress.........: 10592256/14344385 (73.84%)
Rejected.........: 0/10592256 (0.00%)
Restore.Point....: 10586112/14344385 (73.80%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidates.#1....: St0p69692019 -> S0r051x53nt2019
```

- Hashcat also has default rules `/usr/share/hashcat/rules/` but we prefer to use custom rules inorder to reach the target.
- `Hashcat` provides an option to generate random rules on the fly and apply them to the input wordlist. The following command will generate 1000 random rules and apply them to each word from `rockyou.txt` by specifying the "`-g`" flag

```shell-session
htb[/htb]$ hashcat -a 0 -m 100 -g 1000 hash /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt
```

- There is no certainty to the success rate of this attack as the generated rules are not constant.
- There are a variety of publicly available rules as well, such as the [nsa-rules](https://github.com/NSAKEY/nsa-rules), [Hob0Rules](https://github.com/praetorian-code/Hob0Rules), and the [corporate.rule](https://github.com/sparcflow/HackLikeALegend/blob/master/old/chap3/corporate.rule) which is featured in the book [How to Hack Like a Legend](https://www.hacklikeapornstar.com/new-release-hack-like-legend/).
- These are curated rulesets generally targeted at common corporate Windows password policies or based on statistics and probably industry password patter