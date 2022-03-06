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

**SQL Sever Solution** 
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
**SQL Server Pivot Syntax** [Microsoft Docs](https://docs.microsoft.com/en-us/sql/t-sql/queries/from-using-pivot-and-unpivot?view=sql-server-ver15)
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

###**[Binary Tree Nodes](https://www.hackerrank.com/challenges/binary-search-tree-1)**

You are given a table, BST, containing two columns: N and P, where N represents the value of a node in Binary Tree, and P is the parent of N.
| Column | Type    |
| ------ | ------- |
| N      | Integer |
| P      | Integer |

Write a query to find the node type of Binary Tree ordered by the value of the node. Output one of the following for each node:

- Root: If node is root node.
- Leaf: If node is leaf node.
- Inner: If node is neither root nor leaf node.

**Sample Input**

| N   | P    |
| --- | ---- |
| 1   | 2    |
| 3   | 2    |
| 6   | 8    |
| 9   | 8    |
| 2   | 5    |
| 8   | 5    |
| 5   | null |

**Sample Output**

> 1 Leaf
> 2 Inner
> 3 Leaf
> 5 Root
> 6 Leaf
> 8 Inner
> 9 Leaf

**Explanation**
The Binary Tree below illustrates the sample:
<img src="https://s3.amazonaws.com/hr-challenge-images/12888/1443773633-f9e6fd314e-simply_sql_bst.png" alt="Binary Tree" style="height: 120px; width:250px;"/>


**Solution 1**
```sql
SELECT b.n,
CASE 
    WHEN b.p is not null THEN 
        CASE 
            WHEN (SELECT COUNT(*) FROM bst WHERE p = b.n) = 0
            THEN 'Leaf' 
            ELSE 'Inner'
        END
    ELSE 'Root'
END
FROM bst b
ORDER BY b.n   
```

**Solution 2**
```sql
SELECT n, 
IF(p IS NULL,"Root",
   IF((SELECT COUNT(*) FROM bst WHERE p = b.n )>0, "Inner", "Leaf")) 
FROM bst AS b 
ORDER BY n;
```

###**[New Companies](https://www.hackerrank.com/challenges/the-company)**

Amber's conglomerate corporation just acquired some new companies. Each of the companies follows this hierarchy: 
<img src="https://s3.amazonaws.com/hr-challenge-images/19505/1458531031-249df3ae87-ScreenShot2016-03-21at8.59.56AM.png" alt="Hierarchy" style="height: 240px; width:100px;"/>

Given the table schemas below, write a query to print the company_code, founder name, total number of lead managers, total number of senior managers, total number of managers, and total number of employees. Order your output by ascending company_code.

Note:
- The tables may contain duplicate records.
- The company_code is string, so the sorting should not be numeric. For example, if the company_codes are C_1, C_2, and C_10, then the ascending company_codes will be C_1, C_10, and C_2.

**Input Format**

The following tables contain company data:

- Company: The company_code is the code of the company and founder is the founder of the company.

| Column       | Type   |
| ------------ | ------ |
| company_code | String |
| founder      | String |

- Lead_Manager: The lead_manager_code is the code of the lead manager, and the company_code is the code of the working company.

| Column            | Type   |
| ----------------- | ------ |
| lead_manager_code | String |
| company_code      | String |

- Senior_Manager: The senior_manager_code is the code of the senior manager, the lead_manager_code is the code of its lead manager, and the company_code is the code of the working company.

| Column              | Type   |
| ------------------- | ------ |
| senior_manager_code | String |
| lead_manager_code   | String |
| company_code        | String |

- Manager: The manager_code is the code of the manager, the senior_manager_code is the code of its senior manager, the lead_manager_code is the code of its lead manager, and the company_code is the code of the working company.
  
| Column              | Type   |
| ------------------- | ------ |
| manager_code        | String |
| senior_manager_code | String |
| lead_manager_code   | String |
| company_code        | String |

- Employee: The employee_code is the code of the employee, the manager_code is the code of its manager, the senior_manager_code is the code of its senior manager, the lead_manager_code is the code of its lead manager, and the company_code is the code of the working company.
  
| Column              | Type   |
| ------------------- | ------ |
| employee_code       | String |
| manager_code        | String |
| senior_manager_code | String |
| lead_manager_code   | String |
| company_code        | String |

**Sample Input**
Company Table:
| company_code | founder  |
| ------------ | -------- |
| C1           | Monika   |
| C2           | Samantha |

Lead_Manager Table:
| lead_manager_code | company_code |
| ----------------- | ------------ |
| LM1               | C1           |
| LM2               | C2           |

Senior_Manager Table:
| senior_manager_code | lead_manager | company_code |
| ------------------- | ------------ | ------------ |
| SM1                 | LM1          | C1           |
| SM2                 | LM1          | C1           |
| SM3                 | LM2          | C2           |

Manager Table:
| manager_code | senior_manager_code | lead_manager | company_code |
| ------------ | ------------------- | ------------ | ------------ |
| M1           | SM1                 | LM1          | C1           |
| M2           | SM3                 | LM2          | C2           |
| M3           | SM3                 | LM2          | C2           |

Employee Table:
| employee_code | manager_code | senior_manager_code | lead_manager | company_code |
| ------------- | ------------ | ------------------- | ------------ | ------------ |
| E1            | M1           | SM1                 | LM1          | C1           |
| E2            | M1           | SM1                 | LM1          | C1           |
| E3            | M2           | SM3                 | LM2          | C2           |
| E4            | M3           | SM3                 | LM2          | C2           |

**Sample Output**
> C1 Monika 1 2 1 2
> C2 Samantha 1 1 2 2

**Explanation**
In company C1, the only lead manager is LM1. There are two senior managers, SM1 and SM2, under LM1. There is one manager, M1, under senior manager SM1. There are two employees, E1 and E2, under manager M1.

In company C2, the only lead manager is LM2. There is one senior manager, SM3, under LM2. There are two managers, M2 and M3, under senior manager SM3. There is one employee, E3, under manager M2, and another employee, E4, under manager, M3.

**Solution**
```sql
SELECT c.company_code, c.founder, 
    COUNT(DISTINCT lm.lead_manager_code),
    COUNT(DISTINCT sm.senior_manager_code),
    COUNT(DISTINCT m.manager_code),
    COUNT(DISTINCT e.employee_code)
FROM Company c, Lead_manager lm, Senior_Manager sm, Manager m, Employee e
WHERE c.company_code = lm.company_code 
    AND lm.lead_manager_code = sm.lead_manager_code
    AND sm.senior_manager_code = m.senior_manager_code
    AND m.manager_code = e.manager_code
GROUP BY c.company_code, c.founder
ORDER BY c.company_code 
```
