Hashes are, very simply, the product of a cryptographic algorithm to turn a variable length input into a fixed length output.

In MySQL hashes can be used in different ways, for instance to index data into a hash table. Each hash has a unique ID that serves as a pointer to the original data. This creates an index that is significantly smaller than the original data, allowing the values to be searched and accessed more efficiently

However, the data we're going to be extracting are password hashes which are simply a way of storing passwords not in plaintext format.

#### `Additional Resources -->`

Here's some things that might be useful to read after completing this room, if it interests you:

-  [https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/4/html/security_guide/ch-exploits](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/4/html/security_guide/ch-exploits)
-  [https://www.nextgov.com/cybersecurity/2019/10/nsa-warns-vulnerabilities-multiple-vpn-services/160456/](https://www.nextgov.com/cybersecurity/2019/10/nsa-warns-vulnerabilities-multiple-vpn-services/160456/)