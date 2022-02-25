###**[Type of Triangle](https://www.hackerrank.com/challenges/weather-observation-station-4)**

Write a query identifying the type of each record in the TRIANGLES table using its three side lengths. Output one of the following statements for each record in the table:
* **Equilateral**: It's a triangle with sides of equal length.
* **Isosceles**: It's a triangle with sides of equal length.
* **Scalene**: It's a triangle with  sides of differing lengths.
* **Not A Triangle**: The given values of A, B, and C don't form a triangle.

Input Format
The TRIANGLES table is described as follows:

|  Column | Type |
|---|---|
| A  | Integer  |
| B  | Integer  |
| C  | Integer  |

Each row in the table denotes the lengths of each of a triangle's three sides.

**Sample Input**
| A  |  B  |  C  |
| 20 | 20  |  23 |
| 20 | 20  |  20 |
| 20 | 21  |  22 |
| 13 | 14  |  30 |

**Sample Output**
> Isosceles<br>
> Equilateral<br>
> Scalene <br>
> Not A Triangle

**Explanation**

Values in the tuple(20,20,23) form an Isosceles triangle, because A = B.
Values in the tuple(20,20,20) form an Equilateral triangle, because A = B = c . 
Values in the tuple  form a Scalene triangle, because .
Values in the tuple  cannot form a triangle because the combined value of sides  and  is not larger than that of side .

**Solution**
```sql
SELECT COUNT(CITY) - COUNT(DISTINCT CITY) FROM STATION;       
```

###**[Weather Observation Station 5](https://www.hackerrank.com/challenges/weather-observation-station-5)**

Query the two cities in STATION with the shortest and longest CITY names, as well as their respective lengths (i.e.: number of characters in the name). If there is more than one smallest or largest city, choose the one that comes first when ordered alphabetically.

Input Format

The STATION table is described as follows:

|  Field | Type |
|---|---|
| ID  | NUMBER |
| CITY | VARCHAR2(21)   |
| STATE  | VARCHAR2(2)  |
| LAT_N |  NUMBER |
| LONG_W | NUMBER |

where LAT_N is the northern latitude and LONG_W is the western longitude.

*Sample Input*

Let's say that CITY only has four entries: DEF, ABC, PQRS and WXY

*Sample Output*

ABC 3 

PQRS 4

*Explanation*

When ordered alphabetically, the CITY names are listed as ABC, DEF, PQRS, and WXY, with the respective lengths 3,3,4,3,3,4, and 33. The longest-named city is obviously PQRS, but there are 33 options for shortest-named city; we choose ABC, because it comes first alphabetically.

**Solution**
```sql
select CITY,LENGTH(CITY) from STATION order by Length(CITY) asc, CITY limit 1; 
select CITY,LENGTH(CITY) from STATION order by Length(CITY) desc, CITY limit 1;      
```

###**[Weather Observation Station 6](https://www.hackerrank.com/challenges/weather-observation-station-6)**

Query the list of CITY names starting with vowels (a, e, i, o, u) from STATION. Your result cannot contain duplicates.

Input Format

The STATION table is described as follows:

|  Field | Type |
|---|---|
| ID  | NUMBER |
| CITY | VARCHAR2(21)   |
| STATE  | VARCHAR2(2)  |
| LAT_N |  NUMBER |
| LONG_W | NUMBER |

where LAT_N is the northern latitude and LONG_W is the western longitude.

**Solution**
```sql
SELECT DISTINCT city FROM station 
WHERE city LIKE 'A%' 
    OR city LIKE 'E%' 
    OR city LIKE 'I%' 
    OR city LIKE 'O%' 
    OR city LIKE 'U%';      
```

###**[Weather Observation Station 7](https://www.hackerrank.com/challenges/weather-observation-station-7)**

Query the list of CITY names ending with vowels (a, e, i, o, u) from STATION. Your result cannot contain duplicates.

Input Format

The STATION table is described as follows:

|  Field | Type |
|---|---|
| ID  | NUMBER |
| CITY | VARCHAR2(21)   |
| STATE  | VARCHAR2(2)  |
| LAT_N |  NUMBER |
| LONG_W | NUMBER |

where LAT_N is the northern latitude and LONG_W is the western longitude.

**Solution**
```sql
SELECT DISTINCT city FROM station 
WHERE city LIKE '%a' 
    OR city LIKE '%e'
    OR city LIKE '%i'
    OR city LIKE '%o'
    OR city LIKE '%u' ;     
```

###**[Weather Observation Station 8](https://www.hackerrank.com/challenges/weather-observation-station-8/problem)**

Query the list of CITY names from STATION which have vowels (i.e., a, e, i, o, and u) as both their first and last characters. Your result cannot contain duplicates.

Input Format

The STATION table is described as follows:

|  Field | Type |
|---|---|
| ID  | NUMBER |
| CITY | VARCHAR2(21)   |
| STATE  | VARCHAR2(2)  |
| LAT_N |  NUMBER |
| LONG_W | NUMBER |

where LAT_N is the northern latitude and LONG_W is the western longitude.

**Solution**
```sql
SELECT DISTINCT city FROM station 
WHERE (city LIKE 'A%' 
    OR city LIKE 'E%' 
    OR city LIKE 'I%' 
    OR city LIKE 'O%' 
    OR city LIKE 'U%') 
AND (city LIKE '%a' 
    OR city LIKE '%e' 
    OR city LIKE '%i' 
    OR city LIKE '%o' 
    OR city LIKE '%u');     
```

###**[Weather Observation Station 9](https://www.hackerrank.com/challenges/weather-observation-station-9/problem)**

Query the list of CITY names from STATION that do not start with vowels. Your result cannot contain duplicates.

Input Format

The STATION table is described as follows:

|  Field | Type |
|---|---|
| ID  | NUMBER |
| CITY | VARCHAR2(21)   |
| STATE  | VARCHAR2(2)  |
| LAT_N |  NUMBER |
| LONG_W | NUMBER |

where LAT_N is the northern latitude and LONG_W is the western longitude.

**Solution**
```sql
SELECT DISTINCT city FROM station 
WHERE city NOT LIKE 'A%' 
    AND city NOT LIKE 'E%' 
    AND city NOT LIKE 'I%' 
    AND city NOT LIKE 'O%' 
    AND city NOT LIKE 'U%';
```
**others' solution**
```sql
SELECT DISTINCT CITY FROM STATION WHERE upper(SUBSTR(CITY,1,1)) NOT IN ('A','E','I','O','U') AND lower(SUBSTR(CITY,1,1)) NOT IN
('a','e','i','o','u');     
```

###**[Weather Observation Station 10](https://www.hackerrank.com/challenges/weather-observation-station-10/problem)**

Query the list of CITY names from STATION that do not end with vowels. Your result cannot contain duplicates.

Input Format

The STATION table is described as follows:

|  Field | Type |
|---|---|
| ID  | NUMBER |
| CITY | VARCHAR2(21)   |
| STATE  | VARCHAR2(2)  |
| LAT_N |  NUMBER |
| LONG_W | NUMBER |

where LAT_N is the northern latitude and LONG_W is the western longitude.

**Solution**
```sql
SELECT DISTINCT city FROM station
WHERE SUBSTR(city, LENGTH(city), 1) NOT IN ('a', 'e', 'i', 'o', 'u');
```

**other's Solution**
```sql
SELECT DISTINCT CITY FROM STATION WHERE UPPER(SUBSTR(CITY, LENGTH(CITY), 1)) NOT IN ('A','E','I','O','U') AND LOWER(SUBSTR(CITY, LENGTH(CITY),1)) NOT IN ('a','e','i','o','u');    
```

###**[Weather Observation Station 11](https://www.hackerrank.com/challenges/weather-observation-station-11/problem)**

Query the list of CITY names from STATION that either do not start with vowels or do not end with vowels. Your result cannot contain duplicates.

Input Format

The STATION table is described as follows:

|  Field | Type |
|---|---|
| ID  | NUMBER |
| CITY | VARCHAR2(21)   |
| STATE  | VARCHAR2(2)  |
| LAT_N |  NUMBER |
| LONG_W | NUMBER |

where LAT_N is the northern latitude and LONG_W is the western longitude.

**Solution**
```sql
SELECT DISTINCT city FROM station
WHERE UPPER(SUBSTR(city, 1, 1)) NOT IN ('A', 'E', 'I', 'O', 'U')
OR LOWER(SUBSTR(city,-1)) NOT IN ('a', 'e', 'i', 'o', 'u');
```
**Other's Solution**
```sql
SELECT DISTINCT CITY FROM STATION WHERE LOWER(SUBSTR(CITY,1,1)) NOT IN ('a','e','i','o','u') OR LOWER(SUBSTR(CITY, LENGTH(CITY),1)) NOT IN ('a','e','i','o','u');   
```

###**[Weather Observation Station 12](https://www.hackerrank.com/challenges/weather-observation-station-12/problem)**

Query the list of CITY names from STATION that do not start with vowels and do not end with vowels. Your result cannot contain duplicates.

Input Format

The STATION table is described as follows:

|  Field | Type |
|---|---|
| ID  | NUMBER |
| CITY | VARCHAR2(21)   |
| STATE  | VARCHAR2(2)  |
| LAT_N |  NUMBER |
| LONG_W | NUMBER |

where LAT_N is the northern latitude and LONG_W is the western longitude.

**Solution**
```sql
SELECT DISTINCT city FROM station
WHERE UPPER(SUBSTR(city, 1, 1)) NOT IN ('A', 'E', 'I', 'O', 'U')
AND LOWER(SUBSTR(city,-1)) NOT IN ('a', 'e', 'i', 'o', 'u')
```

###**[Higher Than 75 marks](https://www.hackerrank.com/challenges/more-than-75-marks/problem)**

Query the Name of any student in STUDENTS who scored higher than 75 Marks. Order your output by the last three characters of each name. If two or more students both have names ending in the same last three characters (i.e.: Bobby, Robby, etc.), secondary sort them by ascending ID.

Input Format

The STUDENTS table is described as follows:

|  Column | Type |
|---|---|
| ID  | INTEGER |
| NAME | STRING   |
| MARKS  | INTEGER  |

The Name column only contains uppercase (A-Z) and lowercase (a-z) letters.

Sample Input

|  ID | NAME | MARKS |
|---|---|----|
| 1  | Ashley| 81 | 
| 2 | Samantha  | 75 |
| 4  | Julia  |  76 |
| 3  | Julia  |  84 |

**Sample Output**
>Ashley
 Julia
 Belvet

**Explanation**
Only Ashley, Julia, and Belvet have Marks > 75. If you look at the last three characters of each of their names, there are no duplicates and 'ley' < 'lia' < 'vet'.

**Solution**
```sql
SELECT name FROM students
WHERE marks > 75
ORDER BY RIGHT(name, 3), ID ASC
```
**Other's Solution**
```sql
SELECT NAME FROM STUDENTS WHERE MARKS > 75 ORDER BY SUBSTR(NAME, LENGTH(NAME)-2, 3), ID;    
```

###**[Employee Salaries](https://www.hackerrank.com/challenges/salary-of-employees/problem)**

Write a query that prints a list of employee names (i.e.: the name attribute) for employees in Employee having a salary greater than $2000 per month who have been employees for less than 10 months. Sort your result by ascending employee_id.

Input Format

The Employee table containing employee data for a company is described as follows:

|  Column | Type |
|---|---|
| employee_id  | INTEGER |
| name | STRING   |
| months | INTEGER  |
| salary | INTEGER |

where employee_id is an employee's ID number, name is their name, months is the total number of months they've been working for the company, and salary is the their monthly salary.

Sample Input

|  employee_id | name | marks | salary  |
|---|---|----|-----|
| 12228 | Rose | 15 | 1968 |
| 33645 | Angela   | 1 | 3443 |
| 45692  | Frank  | 17  | 1608  |
| 56118  | Patrick  |  7 | 1345
| 59725 | Lisa | 11 | 2330 |
| 74197 | Kimberly   | 16 | 4372 |
| 78454  | Bonnie  |  8 | 1771 |
| 83565 | Michael |  6 | 2017
| 98607  | Todd  |  5 | 3396 |
| 99989 | Joe |  9 | 3573 |

Sample Output

Angela
Michael
Todd
Joe

Explanation

Angela has been an employee for 1 month and earns $3443 per month.
Michael has been an employee for 6 months and earns $2017 per month.
Todd has been an employee for 5 months and earns $3396 per month.
Joe has been an employee for 9 months and earns $3573 per month.
We order our output by ascending employee_id.

**Solution**
```sql
SELECT name FROM employee
WHERE salary > 2000 AND months < 10
ORDER BY employee_id ASC  
```