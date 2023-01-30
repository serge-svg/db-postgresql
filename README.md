<a name="top">
# PostgreSQL tips

## Content index
* [Install PostgreSQL](#toPractice)
* [Documentation](#documentation)
* [Types of Joins](#joins)
* [Date/Time Formatting](#dateTime)   
* [Alter](#alter)   
* [Samples](#sample)   


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

[Index](#top)

<a name="joins">  
  
## Types of Joins
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
SELECT EXTRACT(MONTH FROM payment_date) FROM payment

* Format the full month name
SELECT DISTINCT TO_CHAR(payment_date, 'MONTH') FROM payment

* Format the full day name
SELECT DISTINCT TO_CHAR(payment_date, 'Day') FROM payment

* How many payments occurred on a Monday?
SELECT COUNT(*) FROM payment WHERE EXTRACT(dow FROM payment_date) = 1

[return](#top)
  
<a name="alter">  
  
## Alter
https://www.postgresql.org/docs/current/sql-altertable.html
### ALTER TABLE table_name
ALTER TABLE table_name 
RENAME TO new_table_name

### ALTER COLUMN column_name
ALTER TABLE table_name 
RENAME COLUMN column_name TO new_column_name

### ALTER CONSTRAINT constraint_name
ALTER TABLE table_name ALTER COLUMN column_name DROP NOT NULL
ALTER TABLE students ALTER COLUMN graduation_year TYPE VARCHAR(4)

[return](#top)  

<a name="samples">  

## Samples

### Basic Select
1. Query the two cities in STATION with the shortest and longest CITY names, as well as their respective lengths (i.e.: number of characters in the name).
If there is more than one smallest or largest city, choose the one that comes first when ordered alphabetically.
Note: You can write two separate queries to get the desired output. It need not be a single query.  

SELECT district, LENGTH(district) AS lengthofcity FROM address WHERE LENGTH(district) > 0 ORDER BY lengthofcity, district LIMIT 1;   

2. Query the list of CITY names starting with vowels (i.e., a, e, i, o, or u) from STATION. 
Your result cannot contain duplicates.  
	
![imagen](https://user-images.githubusercontent.com/59533087/212293355-6ef46883-c869-4ff7-81fd-036a5830e6af.png)  
		
SELECT Name FROM Students WHERE Marks > 75 ORDER BY SUBSTR(NAME, LENGTH(Name) -2, 3), ID;

3. Write a query that prints a list of employee names (i.e.: the name attribute) for employees in Employee having a salary greater than per month who have been employees for less than months. Sort your result by ascending employee_id.   
	
![imagen](https://user-images.githubusercontent.com/59533087/212309773-450ed6b0-0ccd-45f6-8ac4-7bb13edf6166.png)  
	
### Advance Select

1. You are given a table, BST, containing two columns: N and P, where N represents the value of a node in Binary Tree, and P is the parent of N.
Write a query to find the node type of Binary Tree ordered by the value of the node. Output one of the following for each node:
    * Root: If node is root node
    * Leaf: If node is leaf node
    * Inner: If node is neither root nor leaf node
  
  ![imagen](https://user-images.githubusercontent.com/59533087/212312206-53af267c-c63c-4f90-8c1c-1deb90f2e098.png)

SELECT N,
CASE 
    WHEN P IS NUll THEN 'Root' 
	  WHEN N IN (SELECT P FROM BST) THEN 'Inner' 
    ELSE 'Leaf'
END Type
FROM BST 
ORDER BY N;

2. Given the table schemas below, write a query to print the company_code, founder name, total number of lead managers, total number of senior managers, total number of managers, and total number of employees. Order your output by ascending company_code.
Note:
* The tables may contain duplicate records.
* The company_code is string, so the sorting should not be numeric. 
* If the company_codes are C_1, C_2, and C_10, then the ascending company_codes will be C_1, C_10, and C_2.  
	
![imagen](https://user-images.githubusercontent.com/59533087/212322649-a8e087ab-a263-496b-995f-76872914af74.png)
![imagen](https://user-images.githubusercontent.com/59533087/212323471-7dc86c83-c222-4898-b2be-951ab3351f05.png)
![imagen](https://user-images.githubusercontent.com/59533087/212323697-8ab95702-d94e-476a-98e8-4d5bcc72478e.png)
![imagen](https://user-images.githubusercontent.com/59533087/212323717-faf4d25a-3d28-4e2c-b7d3-9ff55cd07b7c.png)
![imagen](https://user-images.githubusercontent.com/59533087/212323740-1c6fb4e0-2104-43a0-88a8-16daf7e5ed58.png)
![imagen](https://user-images.githubusercontent.com/59533087/212323755-2a964835-4175-4407-93a2-f4bf8832076e.png)  
	 
    
SELECT e.company_code, c.founder, 
COUNT(distinct e.lead_manager_code), 
COUNT(distinct e.senior_manager_code),
COUNT(distinct e.manager_code),
COUNT(distinct e.employee_code)
FROM Company c 
INNER JOIN employee e ON e.company_code = c.company_code
GROUP BY e.company_code, c.founder 
ORDER BY TO_NUMBER(SUBSTR(c.company_code, 2));    
    
3. Given the CITY and COUNTRY tables, query the names of all the continents (COUNTRY.Continent) and their respective average city populations (CITY.Population) rounded down to the nearest integer.  
Note: CITY.CountryCode and COUNTRY.Code are matching key columns.
![imagen](https://user-images.githubusercontent.com/59533087/212357523-333b0035-1bd8-4887-be17-40d69a7da9f9.png)
![imagen](https://user-images.githubusercontent.com/59533087/212357671-883a964a-38c6-4a49-beef-d05ce6f55457.png)

SELECT COUNTRY.CONTINENT, FLOOR(ROUND(AVG(CITY.POPULATION), 2))
FROM COUNTRY INNER JOIN CITY ON COUNTRY.CODE = CITY.COUNTRYCODE
GROUP BY COUNTRY.CONTINENT;

4. Write a query to generate a report containing three columns: Name, Grade and Mark. Ketty doesn't want the NAMES of those students who received a grade lower than 8. The report must be in descending order by grade -- i.e. higher grades are entered first. If there is more than one student with the same grade (8-10) assigned to them, order those particular students by their name alphabetically. Finally, if the grade is lower than 8, use "NULL" as their name and list them by their grades in descending order. If there is more than one student with the same grade (1-7) assigned to them, order those particular students by their marks in ascending order.  
	
![imagen](https://user-images.githubusercontent.com/59533087/212366293-3e245e6f-110c-42c3-ac3d-4ed9301a0022.png)
![imagen](https://user-images.githubusercontent.com/59533087/212366313-aef56aaa-4121-4c74-a763-ee3976d6a919.png)  
	
SELECT CASE WHEN Grade>=8 THEN Name ELSE 'NULL' END, Grade, Marks
FROM Students s LEFT JOIN Grades g
ON s.marks BETWEEN g.min_mark AND g.max_mark
ORDER BY Grade DESC, name ASC;  

5. Write a query to print the respective hacker_id and name of hackers who achieved full scores for more than one challenge. Order your output in descending order by the total number of challenges in which the hacker earned a full score. If more than one hacker received full scores in same number of challenges, then sort them by ascending hacker_id.  
The following tables contain contest data:
* Hackers: The hacker_id is the id of the hacker, and name is the name of the hacker. 
* Difficulty: The difficult_level is the level of difficulty of the challenge, and score is the score of the challenge for the difficulty level. 
* Challenges: The challenge_id is the id of the challenge, the hacker_id is the id of the hacker who created the challenge, and difficulty_level is the level of difficulty of the challenge. 
* Submissions: The submission_id is the id of the submission, hacker_id is the id of the hacker who made the submission, challenge_id is the id of the challenge that the submission belongs to, and score is the score of the submission.  
	
SELECT S.HACKER_ID, H.NAME FROM SUBMISSIONS S 
INNER JOIN HACKERS H ON H.HACKER_ID = S.HACKER_ID
INNER JOIN CHALLENGES C ON C.CHALLENGE_ID = S.CHALLENGE_ID
INNER JOIN DIFFICULTY D ON D.DIFFICULTY_LEVEL = C.DIFFICULTY_LEVEL
WHERE S.SCORE = D.SCORE
GROUP BY S.HACKER_ID, H.NAME
HAVING COUNT(S.CHALLENGE_ID) > 1
ORDER BY COUNT(S.CHALLENGE_ID) DESC,  S.HACKER_ID ASC;  








