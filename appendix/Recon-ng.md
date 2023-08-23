
***
###### `Title :- Recon-ng`
###### `Created on :- 2023-08-23 - 17:54`
###### `Created by:- Prem J`
***

- [Recon-ng](https://github.com/lanmaster53/recon-ng) is a framework that helps automate the OSINT work.
- It uses modules form various authors and provides a multitude of functionality. Some modules required keys to work; the key allows the module to query the related online API
- You can start Recon-ng by running the command `recon-ng`
- Starting Recon-ng will give you a prompt like `[recon-ng][default] >`. At this stage, you need to select the installed module you want to use
- While it's the first time we use we will need to install modules by creating a ***workspace***

#### `Workspace Creation -->`

- `workspace create thmredteam.com`
- we can also start recon-ng with a predefined workspace `recon-ng -w thmredteam.com`

#### `Seeding the databae -->`

target domain name - thmredteam.com
- we feed it to the Recon-ng database related to active workspace using `db insert domains`

#### `Market Place -->`

- We have a domain name, so a logical next step would be to search for a module that transforms domains into other types of information
- Before you install modules using the marketplace, these are some useful commands related to marketplace usage:

	- `marketplace search KEYWORD` to search for available modules with _keyword_.
	- `marketplace info MODULE` to provide information about the module in question.
	- `marketplace install MODULE` to install the specified module into Recon-ng.
	- `marketplace remove MODULE` to uninstall the specified module.

>[!tip]
>marketplace search is used to get a list of all available modules.

- the naming of modules tells us what kind of information we will get from that transformation
- For instance, doamins-hosts means that the module will find hosts related to the provided domain.
- This module feature works similar to the metasploit payload feature

#### `Working with installed modules -->`

- `modules search` to get a list of all the installed modules
- `modules load module` to load a specific module alt `load module` 
- `options list` list options of the loaded modules
- `options set <option> <value>` 

- [Demo](https://tryhackme.com/room/redteamrecon) of recon-ng

#### `Keys -->`

Some modules cannot be used without a key for the respective service API. `K` indicates that you need to provide the relevant service key to use the module in question.

- `keys list` lists the keys
- `keys add KEY_NAME KEY_VALUE` adds a key
- `keys remove KEY_NAME` removes a key

Once you have the set of modules installed, you can proceed to load and run them.

- `modules load MODULE` loads an installed module
- `CTRL + C` unloads the module.
- `info` to review the loaded module’s info.
- `options list` lists available options for the chosen module.
- `options set NAME VALUE`
- `run` to execute the loaded module.