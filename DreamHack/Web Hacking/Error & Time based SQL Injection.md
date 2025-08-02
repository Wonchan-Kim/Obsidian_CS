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