---
title: How to use SQLite database with Python
description: 
slug: python-sqlite
date: 2022-07-24 00:00:00+0000
image: pexels-tatiana-syrikova-3975585.jpg
categories:
    - DB
tags:
    - Python
    - SQLite
---

Hello🧋  

SQLite is an open source & light database.  
In this post, I will write about how to manage SQLite using Python.


***


### Basic methods to use

>- sqlite3`.connect(dbname)`…Connect to SQLite
>- conn`.cursor()`…Create a cursor object to use SQLite
>- cur`.execute(SQL文)`…Execute an SQL statement 
>- conn`.commit()`…Commit the change
>- conn`.close()`…Close the connection to SQLite

Note: `dbname`, `conn`, and `cur` are variables

### Creating a database and a table
Let’s start making a database called “company” and also a table called “employees”.  
```python
import sqlite3

dbname = 'company.sqlite3'
conn = sqlite3.connect(dbname)
cur = conn.cursor()

cur.execute('CREATE TABLE employees(id INT PRIMARY KEY AUTOINCREMENT, name TEXT, age INT, department TEXT)')

conn.commit()
conn.close()
```  
With this code above, you’ve created a database and a table.

### Inserting a row
By putting SQL statement as an argument  of `cur.execute()`, you can query the database.
This example below is inserting a row about employee data.   
Notice in last example we wrote SQL statement all in one line, but this time we break lines by enclosing them with `'''` (triple quotes).

```python
import sqlite3

dbname = 'company.sqlite3'
conn = sqlite3.connect(dbname)
cur = conn.cursor()

cur.execute(
    '''
    INSERT INTO 
        employees(name, age, department)
    VALUES
        ("ichiro", 50, "IT")
    '''
)

conn.commit()
conn.close()
```

### Inserting a row using variables 
If the value you wish to insert is a variable, you place “?” at the exact spot of where the variable should be in SQL statement. And then write that variable as a second argument of `.execute()`.  
Second argument of `.execute()` needs to be tuple, so if there's just one variable, you need `,` right after it.

```python
import sqlite3

deptname = "Human Resources"

dbname = 'company.sqlite3'
conn = sqlite3.connect(dbname)
cur = conn.cursor()

cur.execute(
    '''
    INSERT INTO 
        employees(name, age, department)
    VALUES
        ("jiro", 32, ?)
    ''',
    (deptname,)
)

conn.commit()
conn.close()
```

***
### Retrieve values
When you want to retrieve data, you use following methods on the cursor objects you’ve made.  
Note: In this post, cursor object is a variable "cur".

>- cur`.fetchone()`…Get one row that matches the requirements 
>- cur`.fetchall()`…Get all rows that matches the requirements 

Example below is a usage of `cur.fetchall()`
```python
import sqlite3

dbname = 'company.sqlite3'
conn = sqlite3.connect(dbname)
cur = conn.cursor()

cur.execute('SELECT * FROM employees')

print(cur.fetchall())

conn.commit()
conn.close()
```
It should print as follows.
```
[(1, 'ichiro', 50, 'IT'), (2, 'jiro', 32, 'Human Resources')]
```

### Retrieve values based on row names
You can get values based on the row name you chose.  
First, you need to set  row_factory attribute of connection object to sqlite3.Row.
>- conn.row_factory = sqlite3.Row

Then, use a for loop to retrieve values based on row names.
>- for row in cur:  
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print(row["input row name here"])

Example usage is as follows:
```python
import sqlite3

dbname = 'company.sqlite3'
conn = sqlite3.connect(dbname)

conn.row_factory = sqlite3.Row

cur = conn.cursor()
cur = conn.execute('SELECT * FROM employees')

for row in cur:
    print(row["name"])

conn.close()
```
It prints like below.
```
ichiro
jiro
```

### References
- [sqlite3 --- DB-API 2.0 interface for SQLite databases](https://docs.python.org/en/3.8/library/sqlite3.html)
