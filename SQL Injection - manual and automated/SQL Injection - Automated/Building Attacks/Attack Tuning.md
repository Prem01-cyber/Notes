
***
###### `Title :- Attack Tuning`
###### `Created on :- 2023-06-22 - 21:48`
###### `Created by:- Prem J`
***

- We can fine tune SQLi injection attempts to help SQLMap to help SQLMap in the detection phase.
- Basically a payload sent to the target by SQLMap consists of

	- ***vector*** (e.g., `UNION ALL SELECT 1,2,VERSION()`): central part of the payload, carrying the useful SQL code to be executed at the target.    
	- ***boundaries*** (e.g. `'<vector>-- -`): prefix and suffix formations, used for proper injection of the vector into the vulnerable SQL statement.

#### `Prefix and Suffix -->`

+ In order to change the boundaries we can add prefix and suffix values, They are not particularly necessary but used rarely.

```bash
sqlmap -u "www.example.com/?q=test" --prefix="%'))" --suffix="-- -"
```

- This will result in an enclosure of all vector values between the static prefix `%'))` and the suffix `-- -`.

```sql
SELECT id,name,surname FROM users WHERE id LIKE (('test%')) UNION ALL SELECT 1,2,VERSION()-- -')) LIMIT 0,1
```


#### `Level and Risk -->`

- By default SQLMap combines a predefined set of risks and boundaries along with vectors having a high chance of success in case of vulnerable target.
- There is always a possibility for users to use bigger set of boundaries and vectors, already incorporated with SQLMap.
- For such demands, the options `--level` and `--risk` should be used

	- The option `--level` (`1-5`, default `1`) extends ***both vectors and boundaries*** being used, based on their expectancy of success (i.e., the lower the expectancy, the higher the level).    
	- The option `--risk` (`1-3`, default `1`) extends the used ***vector set*** based on their risk of causing problems at the target side (i.e., risk of database entry loss or denial-of-service).

>[!tip]
>the best way to check for difference between boundaries and payloads used for different values of level and risk is by using the `-v` verbosity option.

```shell-session
premjampuram@htb[/htb]$ sqlmap -u www.example.com/?id=1 -v 3 --level=5
```

- As for the number of payloads, by default (i.e. `--level=1 --risk=1`), the number of payloads used for testing a single parameter goes up to 72, while in the most detailed case (`--level=5 --risk=3`) the number of payloads increases to 7,865.
- In general SQLMap is tuned to offer best performance and on the default it has the most common vectors and boundaries increasing the level and risk also increases the time for the whole detection process.

#### `Advanced Tuning -->` 

- To further fine-tune the detection mechanism, there is a hefty set of switches and options. In regular cases, SQLMap will not require its usage. Still, we need to be familiar with them so that we could use them when needed.

#### `Table and DB Specification -->`

- If we know which table has our required data we can specify the table using `-T` switch. We can also mention the database using `-D` switch.
- Further to maker it a bit easier we can use the `--batch` and `--dump` options to view the tables in the command line. 

##### `Status Codes ->`

- `--code` switch allows us to see the responses in status codes i.e 200 - `TRUE` and 500 - `FALSE`

##### `Titles ->`

- `--titles` switch allows us to instruct the detection mechanism to bases the comparison based on the content of HTML tag `<title>`.

##### `Strings ->`

- In case of a specific string value appearing in `TRUE` responses (e.g. `success`), while absent in `FALSE` responses, the option `--string` could be used to fixate the detection based only on the appearance of that single value (e.g. `--string=success`).

##### `Text-only ->`

- When dealing with a lot of hidden content, such as certain HTML page behaviors tags (e.g. `<script>`, `<style>`, `<meta>`, etc.), we can use the `--text-only` switch, which removes all the HTML tags, and bases the comparison only on the textual (i.e., visible) content.

##### `Techniques ->`

- Limiting the techniques used by SQLMap with `--technique` switch, which also reduces the detection time. For Example `--technique=BEU`, defaults are specified in [[SQL Injection - manual and automated/SQL Injection - Automated/Getting Started/Introduction|Introduction]] which would be `BEUSTQ`

##### `UNION SQLi Tuning -->`

- Union SQLi is considered as most fastest SQLi.
- In some cases UNION SQLi payloads require extra user provided information to work. If we can manually find the number of columns of the vulnerable SQL Query we can provide this number to SQLMap with `--union-cols`.
- Default value filled by SQLMap is `NULL`, we can change it by specifying our own requirement with `--union-char`

>[!Note]
>Furthermore, in case there is a requirement to use an appendix at the end of a `UNION` query in the form of the `FROM <table>` (e.g., in case of Oracle), we can set it with the option `--union-from` (e.g. `--union-from=users`).
>
>Failing to use the proper `FROM` appendix automatically could be due to the inability to detect the DBMS name before its usage.

>[!tip]
>You can use the option '-T flag5' to only dump data from the needed table. You can use the '--no-cast' flag to ensure you get the correct content. You may also, try running the command a few times to ensure you captured the content correctly.

