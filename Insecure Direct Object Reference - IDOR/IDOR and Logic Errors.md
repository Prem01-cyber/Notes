
- These are called ideas that really doesn't tell us a whole lot, but they're really simple to understand once we get going.
- And so these ideas are going to be just parameters that we can change, that we can so that we can bypass specific functions on a web app with ideas or business logic errors.
- You can bypass payment options, you can reverse payment options so that you're the one getting paid. You can put things into other people's carts.
- There's a lot of different ways you can use IDORs, and when you're out hunting on your own, you're going to have to be creative because there are many different ways that you can exploit IDORs, you can skip login pages, you can access someone else's account.
- The possibilities are endless and the only limit is imagination.

#### `Testing for IDOR -->`

- The best way to test for these, and I think the only way is by creating two different accounts.
- It is always good to create two different accounts because you will have to test in real life against your own accounts.
- You cannot test on someone else's account, so you can't write actually on someone else's wall.
- You don't want to ever mess with the functionality of the application or the people who use it.
- You will only use your own accounts to test against and it also makes it easier to use your own account

- An example or a simple example to remember is to look for ***hidden inputs***.
- You'll actually see this as well in burp.
- When you create an account, it won't say input type hidden, but you will see such as a name and your username and it will say Are you in admin?
- And then it will say. You know, an easy way to show one of these errors is just to change this value to. Yes or true. And you will automatically have admin privileges.

- And one of the other places that you will find, IDORs is in cookies a lot of times.
- A lot of times cookies and tokens will host user information.
- We can remove them and check the authentication process. Burp is the best tool to proceed.
- So that's how websites will keep you authenticated. Not all of them, but some of them will be through cookies.
- Maybe you'll see something in your cookie like user ID and then a number, and you can change the number to something else to see if you can access that account.
- For example, if you had two accounts, you would take the user ID of your victim account and you would replace it on your attacker account and see if you can access parts of that application that you shouldn't under another user.
- If you were getting malicious with this, you would change the value to zero or one and see if you could gain admin privileges to the web app.

- And one of the other places that you will find, IDORs is related to session hijacking.

- Sometimes you will be able to see these in a URL. Like the id field in the url, we may check for errors in it. By changing the value if we are able to access the url it indicates that the website may have an error.

![[Screenshot from 2023-07-09 08-11-19.png]]

- All of this leads to Information disclosure. 