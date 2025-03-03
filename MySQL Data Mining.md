Skills:
[toc]
<br>

### Operators
Operator | Purpose
---------|----------
INTERVAL | 
BINARY, COLLATE |
! |
-,~ |
^ | Bitwise XOR
\*,/,DIV,%,MOD |
-,+ |
<<,>> |
& | Bitwise AND
\| | Bitwise OR
&=,^-=,\|\*= | Compound operators; Bitwise AND, bitwise exclusive equals, bitwise OR equals
=,<=>,>=,>,<=,<,<>,!=, IS, LIKE, REGEXP, IN, MEMBER OF | Comparison operators; <=>/<>/!= all indicate not equal to
BETWEEN, CASE, WHEN, THEN, ELSE | Logical operators
NOT |
AND, && |
XOR |
OR, \|\| |
=,:= |

### Select
``` SQL
SELECT col1 FROM table WHERE col2=value
```
returns values from *col1* in *table* where *col2* is equal to a certain value

### Allow return of null values when filtering with WHERE
logical operators return False for comparisons with null values; use 
``` SQL
WHERE var1 is null;
```
to allow null values

### Removing duplicates, renaming output column, and sorting
``` SQL
SELECT DISTINCT old_col AS new_col FROM table1 WHERE col2=col3 ORDER BY new_col DESC;
```
*DISTINCT* removes duplicates; *AS* renames column name in output pivot table; *ORDER BY* sorts pivot table by *new_col* order; *DESC* dictates order of sorting, default is *ASC* which can be omitted

### Filter by length
``` SQL
SELECT col1 FROM table1 WHERE LENGTH(col2)>val;
```
returns all values from *col1* in *table1* where the length of elements in *col2* is greater than value *val*

### Get values from joined tables
``` SQL
SELECT table1.col1, table2.col3 FROM table1 LEFT OUTER JOIN table2 ON table1.col2=table2.col2
```
returns pivot table with values from *col1* and *col3* by joining corresponding columns in *table1* and *table2*, with correspondence via matching *col2*; use *LEFT OUTER JOIN* to fill null values for *col3* from *table2* which don't have matching value from *col1* in *table1*;
*INNER JOIN* only returns values common to both tables, *OUTER JOIN* fills null values for *table1* if *RIGHT OUTER JOIN* or for *table2* if *LEFT OUTER JOIN*;
MySQL doesn't support *FULL OUTER JOIN*, can emulate via *LEFT* + *RIGHT OUTER JOIN*

### Comparison with previous row
``` SQL
SELECT id, LAG(temperature,1) OVER(ORDER BY recordDate) AS previousDate FROM table1;
```
*LAG()* function returns value from previous *1* row in *temperature* column with table order by *recordDate* as a new column *previousDate*; can perform logical comparisons with new column using *WHERE* for comparisons between rows

### Exhaustive search between elements within row
``` SQL
SELECT w1.id FROM Weather w1, Weather w2 WHERE DATEDIFF(w1.recordDate, w2.recordDate)=1 AND w1.temperature>w2.temperature
```
exhaustively searches combinations of *recordDate* and *temperature* with themselves to match condition

### Connect tables by multiple common columns
``` SQL
SELECT tab1.col1, tab1.col2, tab1.col3, tab2.col3 FROM Table tab1 JOIN Table tab2 ON tab1.col1=tab2.col1 AND tab1.col2=tab2.col2 WHERE tab1.col3=val1 AND tab2.col3=val2
```
joins table on common columns *col1* and *col2* while retaining differing values from *col3* as 2 new columns

### Group by multiple columns
``` SQL
SELECT * FROM table1 GROUP BY col1, col2
```
Groups table by values from both *col1* and *col2*

### Get distinct count of values from column(s)
``` SQL
SELECT id, COUNT(*) AS cnt FROM table1 GROUP BY id;
```
Gets distinct count of values in *id* column
``` SQL
SELECT col1, col2, COUNT(*) AS cnt FROM table1 GROUP BY col1, col2;
```
Gets distinct count of values in *col1* and *col2* columns

### Fill null values with 0
``` SQL
SELECT id, COALESCE(col2,0) FROM table1;
```
Replaces null values in *col2* with 0s

### Pivot row values into new columns
``` SQL
SELECT
	user_id,
	SUM(CASE WHEN action='timeout' THEN 1 ELSE 0 END) AS Timeout,
	SUM(CASE WHEN action='confirmed' THEN 1 ELSE 0 END) AS Confirmed
FROM table1;
```
counts the number of occurrences of *timeout* and *confirmed* in the *action* column and returns them as 2 new columns *Timeout* and *Confirmed*

### Order by multiple columns
``` SQL
SELECT id, percentage FROM t1 ORDER BY id DESC, percentage;
```
Orders table first by *id* then if tie exists, by *percentage*

### Format date
``` SQL
SELECT DATE_FORMAT(date, "%Y-%m") FROM t1;
```
Returns dates as *YYYY-mm* format

### Specify condition in COUNT()
``` SQL
SELECT id, COUNT(IF(condition="True",1,null)) FROM t1;
```
Use *IF* statement to specify a condition in *COUNT*

### Between operator
``` SQL
SELECT id, dates FROM t1 WHERE dates BETWEEN DATE_ADD('2020-02-02',INTERVAL -1 MONTH) AND '2020-02-02'
```
Selects dates between 2 values, inclusive;
Can also use *BETWEEN* with *HAVING*

### Having operator
``` SQL
SELECT id FROM t1 HAVING COUNT(val)>COUNT(SELECT * FROM t2)
```
Having operator allows filtering by specific value via *COUNT*

### Using if-else to return different values based on condition(s)
``` SQL
SELECT id, IF(x>y, 'True', 'False') AS cond FROM table1
```
Returns *True* if value in *x* column greater than *y* else *False*;
Can employ multiple condition in *IF* via *AND*

### Equivalency of multiple values
``` SQL
Select id FROM t1 WHERE val1=val2 AND val2=val3;
```
To compare equivalency between multiple values, must separate into multiple comparison statements connected by *AND* instead of a single statement as *val1=val2=...=valn*

### Sum current row and all previous row values
``` SQL
SELECT id, SUM(col1) OVER(ORDER BY id) AS total_value FROM table1;
```
Returns sum of current row plus all previous row values, as ordered by *id*

### Return only top n of table
``` SQL
SELECT id, val1 FROM table1 ORDER BY val1 DESC LIMIT n;
```
Returns top *n* rows as ordered by *val1* from table;
Can return top *n* least values as ordered by *val1* if in ascending order;
Use *LIMIT n OFFSET m* to get *m*<sup>th</sup> value after *n*

### Set new column header and column values using select
``` SQL
SELECT "first_row" AS col1, val1 FROM table1;
```
Sets new column *col1*\'s value as *first_row*

### Union operator
``` SQL
SELECT id, COUNT(col1<10) AS val1 FROM t1
UNION
SELECT id, COUNT(col1>=10) AS val1 FROM t1;
```
Combines values of two pivot tables with same columns;
Adds rows from second pivot table as rows after first pivot table and keeps ONLY unique values;
Use *UNION ALL* to retain all values

### Multiple case statements
``` SQL
SELECT id, (
	CASE
		WHEN id=Max(id) THEN LEAD(val1) OVER(ORDER BY id)
		WHEN (id<MAX AND id>1)THEN val1
		ELSE LAG(val1) OVER(ORDER BY id)
	END AS val1
)
FROM table1;
```
Specifies different values for separate if/case conditions

### Get ranks of values partitioned by values from another column
``` SQL
SELECT name, department, salary
FROM (
	SELECT *, RANK() OVER(PARTITION BY department ORDER BY salary DESC) AS rank_ord FROM table1
) AS t1
WHERE rank_ord<=10;
```
Returns table with top 10 *salary* for each *department*;
*RANK()* assigns different ranks to the same *ORDER BY* values, ex. two individuals with same salary get different consecutive ranks;
*DENSE_RANK()* assigns same rank to the same *ORDER BY* values, ex. two individuals with same salary get same rank

### Fix formatting of strings
``` SQL
SELECT id, CONCAT(
	UPPER(SUBSTRING(name,1,1)),
	LOWER(SUBSTRING(name,2,LENGTH(name)))
) FROM table1;
```
Capitalizes first letter of *name* and lowercases all remaining letters;
*SUBSTRING()* takes column as 1<sup>st</sup> param, beginning index as 2<sup>nd</sup> param, ending index (inclusive) as 3<sup>rd</sup> param

### Search for substring
``` SQL
SELECT id, name FROM table1 WHERE (name LIKE '%substr%');
```
Searches for columns where *substr* is located in the middle of the string;
*%* allows other string before or after substring

### Returning null in cases where target value doesn't exist
Only functions are capable of returning null when the target can't be found

### Group string values in a column as list separated by commas
``` SQL
SELECT id, GROUP_CONCAT(DISTINCT name) AS names FROM table1 GROUP BY id;
```
Returns distinct names corresponding to each id as a list separated by commas

### Efficiency
- Adding brackets around WHERE conditions increases runtime
- *CASE WHEN...* almost always better than multiple *IF*\'s
