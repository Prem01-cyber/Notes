
***
###### `Title :- Value Fuzzing`
###### `Created on :- 2023-06-06 - 23:41`
###### `Created by:- Prem J`
***

- In the Parameter fuzzing we were the fuzzing the parameter now we are gonna fuzz the **key** using value fuzzing.

#### `Custom Wordlist -->`

- When it comes to fuzzing parameter values, we may not always find a pre-made wordlist that would work for us, as each parameter would expect a certain type of value.
- we could create a wordlist or find a wordlist that is relevant to our task.
- There are many ways to create this wordlist, from manually typing the IDs in a file, or scripting it using Bash or Python. The simplest way is to use the following command in Bash that writes all numbers from 1-1000 to a file:

python --> `for i in $(seq 1 1000);do echo $i >>ids.txt;done`

#### `Value Fuzzing` -->

- The value fuzzing looks similar to parameter (POST) fuzzing.

`ffuf -w ids.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-type: application/x-www-form-urlencoded -fs xxx'`

- The above command represents the basic form for the value fuzzing.

![[Screenshot from 2023-06-07 07-15-19.png]]

- We see that we get a hit right away. We can finally send another `POST` request using `curl`, as we did in the previous section, use the `id` value we just found, and collect the flag.