---
{"dateCreated":"2022-12-15 02:21","tags":["SQL","python"],"pageDirection":"ltr","dg-publish":true,"permalink":"/computer-science/databases/sql-with-python/","dgPassFrontmatter":true}
---


# SQL with Python

python use a standard API to communicate with database named [DB-API](https://peps.python.org/pep-0249/)
the purpose is the following
* encourage similarity between database modules
* common model for all db systems
* connection, cursors, transactions, exception
* SQL remains distinct for each DBMS

__for convenience i will use mysql syntax__ 
before learning how to connect SQL with a different language, it is recommended to read [[Computer Science/Databases/overview of SQL\|overview of SQL]] in order to understand more about the SQL reactive language and how it works.

## connect
the common interface includes a connection object 
``` python
mysql.connector.connect(
 user = "user" ,
 password = "password"
 host = "some-port"
 database = "db-name"
)
```

this will hold the credential for params connection.

## cursor
a cursor object for maintaining context in a sequence of query responses
```python
c = connection.cursor()
c.execute("SELECT * FROM ....")
#iterate over the rows
for row in cur.execute("query"):
	print("row")
```

they serve 2 purposes:
1) iterators to keep track of a position in a database, which allows the code to step through a query result without the need to hold more than one row at a time.
2) in the DB-API implementation cursors also act as prepared statements which allows us to use placeholder in a queries.
``` python 
for row in c.execute("SELECT * FROM Country WHERE region = ?", ("Western Europe")) :
	print(row)
```

## Transaction 
common hooks for committing and rolling back transactions
```python
con.commit()
con.rollback()
```

## Exception
common set of exceptions for catching errors from DBMS 
``` python
try: 
	con.execute("query")
	except mysql.Error:
		print("cannot perform query")
```

## Common Interface
### Executions
when doing `cur.execute('sql_query')` it execute the sql statement but it does not always return any value.
only specific queries return values from the DML of SQL such as `SELECT` 

there are some main ways to fetch the results:
1) use `row = cur.fetchone()` which will fetch one row at a time and than use a while loop to perform some action and the row and than fetch the next one with the same command
``` python
row = cur.fetchone()
while row:
	# some manipulation on row
	row = cur.fetchone()
```

you can fetch all the rows using `rows = rows.fetchall()`

2) use the cursor as an iterator
``` python
cur.execute()
for row in cur: 
	# some manipulation on row
```


