
***
###### `Title :- SQLMap Output Description`
###### `Created on :- 2023-06-21 - 22:46`
###### `Created by:- Prem J`
***

- We received a huge out put in [[Getting Started#`Basic Scenario -->`]] lets break it down.

#### `Log Message Description -->`

- `[22:26:46] [INFO] target URL content is stable` --> This means that there are no changes between responses in cases of continuous identical requests.This is important from the automation point of view since.
- **In the event of stable responses, it is easier to spot differences caused by the potential SQLi attempts**. 
- SQLMap removes the potential noise that could come from the potentially unstable targets

- `[22:26:46] [INFO] GET parameter 'id' appears to be dynamic` --> It is always desired for the tested parameter to be "dynamic," as **it is a sign that any changes made to its value would result in a change in the response**. 
- Indicating that parameter may be linked to the Database.
- In case if the parameter is static and would not change, it indicates that the value of the parameter is not processed by the the target.

>[!tip]
>heuristic means method of detecting upon examining the code and the properties

- `[22:26:46] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')` --> DBMS errors are a good indication of the potential SQLi.
- In this case, there was a MySQL error when SQLMap sends an intentionally invalid value was used (e.g. `?id=1",)..).))'`), which indicates that the tested parameter could be SQLi injectable and that the target could be MySQL.
- It should be noted that this is ***not proof of SQLi***, but just an ***indication that the detection mechanism has to be proven in the subsequent run***.

- `[22:26:46] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting (XSS) attacks` --> SQLMap also runs a ***quick heuristic test*** for the presence of an XSS vulnerability, Not the main purpose but it runs it.

- `it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n] Y` --> Indication of ***what the back end is using***.

- `for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] Y` --> It just basically means running all SQLi payloads for the specific DBMS rather than regular payloads, while In the previous if no DBMS is selected only the top payloads would be tested

- `[22:26:46] [WARNING] reflective value(s) found and filtering out` --> Just a warning that parts of the used payloads are found in the response.
- This behaviour could cause problems to automation tools, as it represents the junk. However, SQLMap has filtering mechanisms to remove such junk before comparing the original page content.
- Similar to scenario where we add payloads as comments. and the added comments gets reflected in the web application.

- `[22:26:46] [INFO] GET parameter 'id' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="luther")`--> Indicates parameter is injectable, though there is still a chance for it to be a false-positive finding
- In case of boolean-based blind SQLi if SQLMap find s high chance of false-positives, at the end run SQLMap performs extensive simple checks consisting of simple logic checks for the removal of false-positives.
- Additionally, `with --string="luther"` indicates that SQLMap recognized and used the appearance of constant string value `luther` in the response for distinguishing `TRUE` from `FALSE` responses.
- This is an important finding because in such cases, there is no need for the usage of advanced internal mechanisms, such as dynamicity/reflection removal or fuzzy comparison of responses, which cannot be considered as false-positive.

- `[22:26:46] [WARNING] time-based comparison requires larger statistical model, please wait........... (done)` --> SQLMap uses a statistical model for the recognition of regular and (deliberately) delayed target responses.
- For this model to work, there is a requirement to collect a sufficient number of regular response times. This way SQLMap can distinguish between the deliberate delay and even in the high latency network environments.

>[!tip]
>Network latency is the delay in network communication. It shows the time that data takes to transfer across the network. **Networks with a longer delay or lag** have high latency, while those with fast response times have low latency

- `[22:26:56] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found` --> UNION-query SQLi checks require considerably more requests for successful recognition of usable payload than other SQLi types.
- To lower the testing time per parameter, especially if the target does not appear to be injectable, the number of requests is capped to a constant value (i.e., 10) for this type of check
- However, if there is a good chance that the target is vulnerable, especially as one other (potential) SQLi technique is found, SQLMap extends the default number of requests for UNION query SQLi, because of a higher expectancy of success.

- `[22:26:56] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test` --> As a heuristic check for the UNION-query SQLi type, before the actual `UNION` payloads are sent.
- In case that it is usable, SQLMap can quickly recognize the correct number of required `UNION` columns by conducting the binary-search approach.
- **Note that this depends on the affected table in the vulnerable query.**

- `GET parameter 'id' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N` --> The parameter is vulnerable to SQLi. If we want an extensive report we can continue else and we can just perform exploitation on that parameter.

- `sqlmap identified the following injection point(s) with a total of 46 HTTP(s) requests:` --> Upon identifying the vulnerable field now the tool performs SQLi using injection techniques which represents the final proof of successful detection of SQLi.
- It should be noted that SQLMap lists only those findings which are provably exploitable (i.e., usable).

- `[22:26:57] [INFO] fetched data logged to text files under '/home/user/.sqlmap/output/www.example.com'` --> indicates the local file system used for storing logs, sessions, output data for a specific target - in this case `www.example.com` 
- After such an initial run, where the injection point is successfully detected, all details for future runs are stored inside the same directory's session files. This means that SQLMap tries to reduce the required target requests as much as possible, depending on the session files' data.