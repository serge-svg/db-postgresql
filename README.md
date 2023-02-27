<a name="top">
# PostgreSQL tips

## Content index
* [Install PostgreSQL](#toPractice)
* [Documentation](#documentation)
* [Joins](#joins)
* [Commands](#commands)
* [Date/Time Formatting](#dateTime)   
* [Alter](#alter)   


<a name="toPractice">  

## To practice (optinal environment)
* Install postgreSQL
* Load databases
	* Download data base rentaldvd.tar  
	* Download data base exercises.tar 
* Exercises
	* https://docs.google.com/document/d/1wiuYbTQslmfolQWgeVPB356csjK6yqOUBhgC7fM44o8/edit

<a name="documentation">  
  
## Documentation
* https://www.postgresql.org/docs/current/index.html  

[return](#top)

<a name="joins">  
  
## Joins
<p align="center">
  <img src="https://user-images.githubusercontent.com/59533087/211632361-3d17caa8-d1a6-4b35-af49-6656485770f8.png">
</p>

* PostgreSQL INNER JOIN (or sometimes called simple join)
  * It is the most common type of join, INNER JOINS return all rows from multiple tables where the join condition is met.
* PostgreSQL LEFT OUTER JOIN (or sometimes called LEFT JOIN)
  * This type of join returns all rows from the LEFT-hand table specified in the ON condition and only those rows from the other table where the joined           fields are equal (join condition is met).
* PostgreSQL RIGHT OUTER JOIN (or sometimes called RIGHT JOIN)
  * This type of join returns all rows from the RIGHT-hand table specified in the ON condition and only those rows from the other table where the joined         fields are equal (join condition is met).
* PostgreSQL FULL OUTER JOIN (or sometimes called FULL JOIN)
  * This type of join returns all rows from the LEFT-hand table and RIGHT-hand table with nulls in place where the join condition is not met.

<a name="commands"> 
	
## Commands
<p align="center">
  <img src="https://user-images.githubusercontent.com/59533087/221257371-db4e79f6-3cd5-4b3b-87fc-ad5077319957.png">
</p>

[return](#top)

<a name="dateTime">  
    
## Date/Time Formatting
https://www.postgresql.org/docs/12/functions-formatting.html    
SELECT NOW()  
SELECT TIMEOFDAY()  
SELECT CURRENT_DATE  
SELECT AGE(last_update) FROM actor  
SELECT EXTRACT(YEAR FROM payment_date) AS year FROM payment  
SELECT EXTRACT(QUARTER FROM payment_date) AS year FROM payment  
SELECT TO_CHAR(payment_date, 'DD-MM-YYYY') FROM payment  
SELECT TO_CHAR(payment_date, 'MONTH YYYY') FROM payment  
SELECT TO_CHAR(payment_date, 'mon / YYYY') FROM payment  

* During which months did payments occur?
	* SELECT EXTRACT(MONTH FROM payment_date) FROM payment

* Format the full month name
	* SELECT DISTINCT TO_CHAR(payment_date, 'MONTH') FROM payment

* Format the full day name
	* SELECT DISTINCT TO_CHAR(payment_date, 'Day') FROM payment

* How many payments occurred on a Monday?
	* SELECT COUNT(*) FROM payment WHERE EXTRACT(dow FROM payment_date) = 1

[return](#top)
  
<a name="alter">  
  
## Alter
https://www.postgresql.org/docs/current/sql-altertable.html
* ALTER TABLE table_name
	* ALTER TABLE table_name RENAME TO new_table_name

* ALTER COLUMN column_name
	* ALTER TABLE table_name RENAME COLUMN column_name TO new_column_name

* ALTER CONSTRAINT constraint_name
	* ALTER TABLE table_name ALTER COLUMN column_name DROP NOT NULL
	* ALTER TABLE students ALTER COLUMN graduation_year TYPE VARCHAR(4)

[return](#top)  





