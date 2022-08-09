---
title: What is hashing & salting? How to store the password safely
description: 
slug: hash-and-salt
date: 2022-06-08 00:00:00+0000
image: hash.jpg
categories:
    - Security
tags:
    - password
    - hashing
    - bcrypt
    - Python
---

Hello🌱  
In this post, I'm going to write about how to store passwords in database.

***
### Why you should NOT store passwords as it is on the database
It is a common procedure to set a user name and password when you register to any kind of web service.  
User name and password will be sent to a server, then stored in a database.  

But what if this data gets stolen by attackerks...?

> 😎🗯I'm going to do some malicious things using the user name and the password!

This would be such a mess.  
It is very risky to store passwords as plain text when you consider a situation like this. 

So, how should you store passwords then? 
***

### Hashing
A useful ways to store passwords securely is by **hashing**.  
　　　
#### What is it?
- Hashing is a process of **transforming given data into a random string** using mathematical algorithm called hash function. 
- Hashing algorithm has some different types. e.g. MD5 and SHA256
- It is **nearly impossible to revert** the hashed text to original input.
- Same input resuls in same outputs.  
  
#### Try it out
Let's use a hashing algorithm called SHA256 on Python to hash a password.  
1. Import `hashlib` from standard library.  
2. Assign the password to a variable `password`.
3. `encode()` it to convert text string into byte string.
4. Set it as an argument of `hashlib.sha256()`.   
5. Convert it into hexadecimal digits by `.hexdigest()`
```python {.scroll}
>>> import hashlib
>>> password = "apple"
>>> hashed = hashlib.sha256(password.encode()).hexdigest()
>>> print(hashed)
```
Print result will be ...
```
3a7bd3e2360a3d29eea436fcfb7e44c735d117c42d1c1835420b6b9942dd4f1b
```

Now nobody can tell what the password was!  

Let's try using a dfferent text as a password this time.  
```python
>>> password = "りんご"
>>> hashed = hashlib.sha256(password.encode()).hexdigest()
>>> print(hashed)
```
Output is...
```
4261abfc91324dc5319312592125610a16b0b0a996fcdfae1d24766b918afae9
```
This time we got a different output!  
While the outputs were different, their length were the same. Hashing transforms different length of inputs to fixed length of output.

***
### A porblem of hashing passwords
We understood that hashing is secure, but not quite enough.  

For example, let's think of a story that Alice made an account on an app.  
This time she was busy, and she registered her password as "password123".  
Meanwhile, Bob and Carol was using the exact same password in coincidence.  
Then what happens?

>👩 "password123"<br>  
>👨 "password123"<br>  
>👵 "password123"<br>  

Everyone's password got hashed then saved, but since their original strings are the same, their hashed ones are also same.  
As a result, people can tell that these three people are using the same password. This means that if one of their passwords gets leaked, the others are also leaked.

>👩 "password123"  →  "ef92b778bafe771e..."<br>  
>👨 "password123"  →  "ef92b778bafe771e..."<br>   
>👵 "password123"  →  "ef92b778bafe771e..."<br>    
>😎💭 These guys are using the same password... I see...
 
Some attackers try to guess the password by brute force attack which is an attempt to find a password by trying every possible combination of letters, numbers, etc.  
You would wanna take some measure so that it'll be safe even if one password gets leaked.

***
### Salting
One solution for this is **salting** passwords.
>😧💭 Salting... like sodium🧂...?  

Yes, it is like sprinkling some salt on hashed potato.(Just my point of view😂)

#### What is salting?
 - Salt is a random string that you add to the original data.
 - Use different salt for sifferent data.
 - By salting each data before hashing, you get a different results from the same data.

This helps storing passwords more securely.
***
### Use bcrypt
By using a library called [bcrypt](https://pypi.org/project/bcrypt/), it would be much easier to create a salted & hashed password. Let's start using it by `pip install`. 
#### Try it out
- `bcrypt.gensalt()`  
...Generate a random salt 
- `bcrypt.hashpw([password],[salt])`  
...Add salt to password, then hash it

Following is an example of using two methods above.
```python
>>> import bcrypt
>>> password = "opensesame01"
>>> hashed_and_salted = bcrypt.hashpw(bytes(password, 'utf-8'), bcrypt.gensalt())
```

Then, print `hashed_and_salted`...

```python
>>> print(hashed_and_salted)
b'$2b$12$j6xy952qXR3xCYa8SOpGiewm5DXAZ/YNfbgJHcwuOkjml7jZDkxUi'
```
Yay! You created a salted & hashed string.  

Note: An argument of `.hashpw()` needs to be an byte string. Therefore you need to use `bytes(password, 'utf-8')`like above to convert it into byte string.
Other ways to convert text string to byte string are `encode([string])`and`b"[string]"`. These converts into UTF-8 as a default.
***
Let's compare it with the samepassword but **just hashed**.
```python
>>> import hashlib
>>> password = "opensesame01"
>>> hashed = hashlib.sha256(password.encode()).hexdigest()
>>> print(hashed)
'4b368659a35f63cef458a1811e46513da66e62ca27c9a31f613bd62189b0e1ed'
```
Different string has been generated!  

Lastly, imagine you're logging in a web service using password, and let's check if password stored in the database is the same as password you entered.  
When checking, put a string you want to check as a first argument, and a hashed password (converted password atored in DB) as a second argument.  
- `bcrypt.checkpw([entered_password],[hashed_password])`

```python
>>> entered_password = "opengoma01"
>>> bcrypt.checkpw(bytes(entered_password, 'utf-8'), hashed_and_salted)
False
>>> entered_password = "opensesame01"
>>> bcrypt.checkpw(bytes(entered_password, 'utf-8'), hashed_and_salted)
True
```
As it's shown above, it returned false when entered_password is wrong, and True when it matches!

> 👩👨👵 Yaaay!

***  
### Let's securely store passwords by hashing and salting!
  
### References
>- [hashlib — Secure hashes and message digests](https://docs.python.org/3/library/hashlib.html)  

🍔🧂🍟
