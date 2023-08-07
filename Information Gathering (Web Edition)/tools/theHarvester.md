
***
###### `Title :- TheHarvester`
###### `Created on :- 2023-05-27 - 19:53`
###### `Created by:- Prem J`
***
#### `Brief -->`

- [TheHarvester](https://github.com/laramies/theHarvester) is a simple yet so powerful tool for early stages of pen-testing.

#### `What does it collect -->`

- It collects **emails names subdomains IP addresses and URLs** from various public data sources for passive information gathering.
- Some of the sources have been mentioned in [[information_gathering_tools]].

#### `Commands -->`

- We can automate the we copy the publicly available source list into a text file (sources.txt)

>	`cat sources.txt | while read source; do theHarvester -d "${TARGET}" -b $source -f "${source}_${TARGET}";done

- When the process finishes we can extract the required information 

>	`cat *.json | jq -r '.hosts[]' 2>/dev/null | cut -d':' -f 1 | sort -u > "${TARGET}_theHarvester.txt"`

- At the end we can merge all the passive reconnaissance files

>	`cat facebook.com_*.txt | sort -u > facebook.com_subdomains_passive.txt`