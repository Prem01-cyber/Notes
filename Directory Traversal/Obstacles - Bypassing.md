
***
###### `Title :- Obstacles`
###### `Created on :- 2023-07-12 - 07:33`
###### `Created by:- Prem J`
***

- While performing directory it is not common to see that application has no defence against the directory traversal.
- Many applications that place user input into file paths implement some kind of defence against path traversal attacks, and these can often be circumvented.

#### `Possible alternatives -->`

### `1. Absolute Path -->`

- You might be able to use an absolute path from the filesystem root, such as `filename=/etc/passwd`, to directly reference a file without using any traversal sequences.

### `2. Nested Traversal Sequences -->`

- You might be able to use nested traversal sequences, such as `....//` or `....\/`, which will revert to simple traversal sequences when the inner sequence is stripped.

>[!Note]
>In some contexts, such as in a URL path or the `filename` parameter of a `multipart/form-data` request, web servers may strip any directory traversal sequences before passing your input to the application. You can sometimes bypass this kind of sanitization by URL encoding, or even double URL encoding, the `../` characters, resulting in `%2e%2e%2f` or `%252e%252e%252f` respectively. Various non-standard encoding's, such as `..%c0%af` or `..%ef%bc%8f`, may also do the trick.

- If an application requires that the user-supplied filename ***must end with an expected file extension***, such as `.png`, then it might be possible to use a null byte to effectively terminate the file path before the required extension. For example: `filename=../../../etc/passwd%00.png`