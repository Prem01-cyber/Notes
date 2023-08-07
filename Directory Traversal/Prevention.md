
***
###### `Title :- Prevention`
###### `Created on :- 2023-07-12 - 07:40`
###### `Created by:- Prem J`
***

- The most effective way to prevent file path traversal vulnerabilities is to ***avoid passing user-supplied input*** to filesystem APIs altogether.
- Many application functions that do this can be rewritten to deliver the same behavior in a safer way
- If it is considered unavoidable to pass user-supplied input to filesystem APIs, then two layers of defense should be used together to prevent attacks:

1. The application should validate the user input before processing it, i.e there should be a set of permitted values and the user input must be validated against it.
2. After validating the supplied input, the application should append the input to the base directory and use a platform filesystem API to canonicalize the path. it should be verified that the canonicalzed path starts with the expected base directory

```java
File file = new File(BASE_DIRECTORY, userInput); if (file.getCanonicalPath().startsWith(BASE_DIRECTORY)) { // process file }
```