###**[Type of Triangle](https://www.hackerrank.com/challenges/what-type-of-triangle)**

Write a query identifying the type of each record in the TRIANGLES table using its three side lengths. Output one of the following statements for each record in the table:
* **Equilateral**: It's a triangle with sides of equal length.
* **Isosceles**: It's a triangle with sides of equal length.
* **Scalene**: It's a triangle with  sides of differing lengths.
* **Not A Triangle**: The given values of A, B, and C don't form a triangle.

Input Format
The TRIANGLES table is described as follows:

| Column | Type    |
| ------ | ------- |
| A      | Integer |
| B      | Integer |
| C      | Integer |

Each row in the table denotes the lengths of each of a triangle's three sides.

**Sample Input**
| A   | B   | C   |
| --- | --- | --- |
| 20  | 20  | 23  |
| 20  | 20  | 20  |
| 20  | 21  | 22  |
| 13  | 14  | 30  |

**Sample Output**
> Isosceles<br>
> Equilateral<br>
> Scalene <br>
> Not A Triangle

**Explanation**

Values in the tuple(20,20,23) form an Isosceles triangle, because A = B. <br>
Values in the tuple(20,20,20) form an Equilateral triangle, because A = B = c . <br>
Values in the tuple(20,21,22) form a Scalene triangle, because A != B != C.<br>
Values in the tuple(13, 14, 30) cannot form a triangle because the combined value of sides A and B is not larger than that of side C.<br>

**Solution**
```sql
SELECT 
CASE
WHEN A + B <= C OR A + C <= B or B + C <= A THEN "Not A Triangle"
WHEN A = B AND B = C THEN "Equilateral"
WHEN A = B OR A = C OR B = C THEN "Isosceles"
ELSE "Scalene"
END AS 'triangle_sides'
FROM TRIANGLES 
```

###**[The PADS](https://www.hackerrank.com/challenges/the-pads)**

Generate the following two result sets:

Query an alphabetically ordered list of all names in OCCUPATIONS, immediately followed by the first letter of each profession as a parenthetical (i.e.: enclosed in parentheses). For example: AnActorName(A), ADoctorName(D), AProfessorName(P), and ASingerName(S).
Query the number of ocurrences of each occupation in OCCUPATIONS. Sort the occurrences in ascending order, and output them in the following format:
> There are a total of [occupation_count] [occupation]s.

where [occupation_count] is the number of occurrences of an occupation in OCCUPATIONS and [occupation] is the lowercase occupation name. If more than one Occupation has the same [occupation_count], they should be ordered alphabetically.

**Note**: There will be at least two entries in the table for each type of occupation.

**Input Format**

The **OCCUPATIONS** table is described as follows:

| Column     | Type   |
| ---------- | ------ |
| Name       | String |
| Occupation | String |

 Occupation will only contain one of the following values: **Doctor**, **Professor**, **Singer** or **Actor**.

**Sample Input**

| Name      | Occupation |
| --------- | ---------- |
| Samatha   | Doctor     |
| JUlia     | Actor      |
| Maria     | Actor      |
| Meera     | Signer     |
| Ashely    | Professor  |
| Ketty     | Professor  |
| Christeen | Professor  |
| Jane      | Actor      |
| Jenny     | Doctor     |
| Priya     | Singer     |

**Sample Output**
> Ashely(P) <br>
> Christeen(P)<br>
> Jane(A)<br>
> Jenny(D)<br>
> Julia(A)<br>
> Ketty(P)<br>
> Maria(A)<br>
> Meera(S)<br>
> Priya(S)<br>
> Samantha(D)<br>
> There are a total of 2 doctors.<br>
> There are a total of 2 singers.<br>
> There are a total of 3 actors.<br>
> There are a total of 3 professors.<br>

**Explanation**

The results of the first query are formatted to the problem description's specifications.<br>
The results of the second query are ascendingly ordered first by number of names corresponding to each profession (2<=2<=2<=3<=3), and then alphabetically by profession (doctor<=singer, and actor<=professor).

**Solution**
```sql
SELECT CONCAT(name, "(", SUBSTR(occupation, 1, 1), ")")
FROM occupations
ORDER BY name ASC;
SELECT CONCAT("There are a total of ", COUNT(occupation), " ", LOWER(occupation), "s.") 
FROM occupations
GROUP BY occupation
ORDER BY COUNT(occupation), LOWER(occupation);     
```

###**[Occupations](https://www.hackerrank.com/challenges/occupations/)**

Pivot the Occupation column in OCCUPATIONS so that each Name is sorted alphabetically and displayed underneath its corresponding Occupation. The output column headers should be Doctor, Professor, Singer, and Actor, respectively.

Note: Print **NULL** when there are no more names corresponding to an occupation.

**Input Format**

The **OCCUPATIONS** table is described as follows:

| Column     | Type   |
| ---------- | ------ |
| Name       | String |
| Occupation | String |

Occupation will only contain one of the following values: **Doctor**, **Professor**, **Singer** or **Actor**.

**Sample Input**

| Name      | Occupation |
| --------- | ---------- |
| Samatha   | Doctor     |
| JUlia     | Actor      |
| Maria     | Actor      |
| Meera     | Signer     |
| Ashely    | Professor  |
| Ketty     | Professor  |
| Christeen | Professor  |
| Jane      | Actor      |
| Jenny     | Doctor     |
| Priya     | Singer     |

**Sample Output**
> Jenny    Ashley     Meera  Jane <br>
> Samantha Christeen  Priya  Julia <br>
> NULL     Ketty      NULL   Maria <br>

**Explanation**

The first column is an alphabetically ordered list of Doctor names.<br>
The second column is an alphabetically ordered list of Professor names.<br>
The third column is an alphabetically ordered list of Singer names.<br>
The fourth column is an alphabetically ordered list of Actor names.<br>
The empty cell data for columns with less than the maximum number of names per occupation (in this case, the Professor and Actor columns) are filled with NULL values.<br>

**SQL Sever Solution** [Tutorial](https://www.techonthenet.com/sql_server/pivot.php)
```sql
SELECT [doctor], [professor], [singer], [actor]
FROM
    (SELECT 
         ROW_NUMBER() OVER (PARTITION BY occupation ORDER BY name) row_num, 
         name, occupation
     FROM 
         Occupations
    ) AS tmp_table 
PIVOT
    (MIN(name) 
     FOR occupation IN ([doctor], [professor], [singer], [actor])
    ) AS pivot_table 
ORDER BY row_num;
```
**SQL Server Pivot Syntax**
```sql
SELECT <non-pivoted column>,  
    [first pivoted column] AS <column name>,  
    [second pivoted column] AS <column name>,  
    ...  
    [last pivoted column] AS <column name>  
FROM  
    (<SELECT query that produces the data>)   
    AS <alias for the source query>  
PIVOT  
(  
    <aggregation function>(<column being aggregated>)  
FOR   
[<column that contains the values that will become column headers>]   
    IN ( [first pivoted column], [second pivoted column],  
    ... [last pivoted column])  
) AS <alias for the pivot table>  
<optional ORDER BY clause>;
```

**MySQL Solution**
```sql
SET @doc=0, @prof=0, @singer=0, @actor=0;

SELECT MIN(doc_names), MIN(prof_names), MIN(singer_names), MIN(actor_names)
FROM(
    SELECT 
    CASE
        WHEN occupation = 'Doctor' THEN (@doc := @doc+1)
        WHEN occupation = 'Professor' THEN (@prof := @prof+1)
        WHEN occupation = 'Singer' THEN (@singer := @singer+1)
        WHEN occupation = 'Actor' THEN (@actor := @actor+1)
     END AS row_num,
     CASE WHEN occupation = 'Doctor' THEN name END AS doc_names,
     CASE WHEN occupation = 'Professor' THEN name END AS prof_names,
     CASE WHEN occupation = 'Singer' THEN name END AS singer_names,
     CASE WHEN occupation = 'Actor' THEN name END AS actor_names
     FROM occupations
     ORDER BY name) AS tmp_table
GROUP BY row_num;
```

###**[Weather Observation Station 7](https://www.hackerrank.com/challenges/weather-observation-station-7)**

Query the list of CITY names ending with vowels (a, e, i, o, u) from STATION. Your result cannot contain duplicates.

Input Format

The STATION table is described as follows:

| Field  | Type         |
| ------ | ------------ |
| ID     | NUMBER       |
| CITY   | VARCHAR2(21) |
| STATE  | VARCHAR2(2)  |
| LAT_N  | NUMBER       |
| LONG_W | NUMBER       |

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

| Field  | Type         |
| ------ | ------------ |
| ID     | NUMBER       |
| CITY   | VARCHAR2(21) |
| STATE  | VARCHAR2(2)  |
| LAT_N  | NUMBER       |
| LONG_W | NUMBER       |

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

| Field  | Type         |
| ------ | ------------ |
| ID     | NUMBER       |
| CITY   | VARCHAR2(21) |
| STATE  | VARCHAR2(2)  |
| LAT_N  | NUMBER       |
| LONG_W | NUMBER       |

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

| Field  | Type         |
| ------ | ------------ |
| ID     | NUMBER       |
| CITY   | VARCHAR2(21) |
| STATE  | VARCHAR2(2)  |
| LAT_N  | NUMBER       |
| LONG_W | NUMBER       |

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

| Field  | Type         |
| ------ | ------------ |
| ID     | NUMBER       |
| CITY   | VARCHAR2(21) |
| STATE  | VARCHAR2(2)  |
| LAT_N  | NUMBER       |
| LONG_W | NUMBER       |

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

| Field  | Type         |
| ------ | ------------ |
| ID     | NUMBER       |
| CITY   | VARCHAR2(21) |
| STATE  | VARCHAR2(2)  |
| LAT_N  | NUMBER       |
| LONG_W | NUMBER       |

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

| Column | Type    |
| ------ | ------- |
| ID     | INTEGER |
| NAME   | STRING  |
| MARKS  | INTEGER |

The Name column only contains uppercase (A-Z) and lowercase (a-z) letters.

Sample Input

| ID  | NAME     | MARKS |
| --- | -------- | ----- |
| 1   | Ashley   | 81    |
| 2   | Samantha | 75    |
| 4   | Julia    | 76    |
| 3   | Julia    | 84    |

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

| Column      | Type    |
| ----------- | ------- |
| employee_id | INTEGER |
| name        | STRING  |
| months      | INTEGER |
| salary      | INTEGER |

where employee_id is an employee's ID number, name is their name, months is the total number of months they've been working for the company, and salary is the their monthly salary.

Sample Input

| employee_id | name     | marks | salary |
| ----------- | -------- | ----- | ------ |
| 12228       | Rose     | 15    | 1968   |
| 33645       | Angela   | 1     | 3443   |
| 45692       | Frank    | 17    | 1608   |
| 56118       | Patrick  | 7     | 1345   |
| 59725       | Lisa     | 11    | 2330   |
| 74197       | Kimberly | 16    | 4372   |
| 78454       | Bonnie   | 8     | 1771   |
| 83565       | Michael  | 6     | 2017   |
| 98607       | Todd     | 5     | 3396   |
| 99989       | Joe      | 9     | 3573   |

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