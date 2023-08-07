
***
###### `Title :- Introduction`
###### `Created on :- 2023-06-05 - 07:22`
###### `Created by:- Prem J`
***

- There are many tools that are used to utilise or directory and parameter **fuzzing**. ffuf ([[cheatsheet-54]]) is one of the tools used for this and we will be seeing it.3.
- Tools such as ffuf provide us with a handy automated way to fuzz the web application's individual components or a web page.

#### `Fuzzing -->`

- Fuzzing can be refereed as sending various types of requests with slightly altered values to study how it reacts automatically (By checking the HTTP code).
- if we were fuzzing for SQL injection --> sending random special characters and see how the server reacts.
- if we were fuzzing for buffer overflow --> sending long strings and incrementing their length to see it when the binary would work.
- [[burpsuite]] Intruder also performs the same task.
- We usually utilize pre-defined wordlists of commonly used terms for each type of test for web fuzzing to see if the webserver would accept them.
- This is done because web servers do not usually provide a directory of **all available links** and domains (unless terribly configured), and so we would have to check for various links and see which ones **return pages**.

#### `Wordlists -->`

- Many admins follow a pattern naming the web pages, yet some of them are made with unique names which makes them difficult to find.
- Wordlists for web fuzzing have already been made, some of the most commonly used wordlists can be found under [Seclist](https://github.com/danielmiessler/SecLists). Which has wordlists under various types of fuzzing and even more categories.

>[!Tip]
>Taking a look at this wordlist we will notice that it contains copyright comments at the beginning, which can be considered as part of the wordlist and clutter the results. We can use the following command to get rid of these lines with the `-ic` flag (In ffuf).



