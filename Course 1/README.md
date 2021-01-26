# Intro to SQL
### Query Selection:
Keywords:
1. SELECT
2. FROM
3. DISTINCT  ( Unique entries/ records from the columns )
4. COUNT ( count the number of rows in one/more columns )
	>  SELECT COUNT(*) FROM table;
	**we can check for total number of rows in a table with this code**
5. WHERE (Selective search based on conditions)
6. AND (for using more than 1 conditions together , eg WHERE ... AND ... )
7. OR ( for meeting **at least one** of the conditions getting fulfilled, for eg WHERE ... OR ...  etc. )
8. BETWEEN
9. IN
10. NULL / NOT NULL
11.  LIKE / NOT LIKE 
12. AVG()
13. MAX()
14. SUM() 
15.  AS (aliasing)
16.  ORDER BY
17.  GROUP BY
18.  HAVING
19.  LIMIT (limit the number of rows returned)

## Important Points

* It is also common to combine `COUNT` with `DISTINCT` to count the number of distinct values in a column.
	> SELECT COUNT( DISTINCT column_name ) FROM table;
* **Important: in PostgreSQL (the version of SQL we're using), you must use single quotes with  `WHERE`.**
* You can link as many `AND` or `OR` after `WHERE` in the conditions. There is no limit.
* Checking for ranges like (<= AND >= or etc.) is very common, so in SQL the  `BETWEEN`  keyword provides a useful shorthand for filtering values within a specified range. This query is equivalent to the one above:

```
SELECT title
FROM films
WHERE release_year
BETWEEN 1994 AND 2000;

```

It's important to remember that  `BETWEEN`  is  _inclusive_, meaning the beginning and end values are included in the results!
* The  `IN`  operator allows you to specify multiple values in a  `WHERE`  clause, making it easier and quicker to specify multiple  `OR`  conditions! Neat, right?

So, the above example would become simply:

```
SELECT name
FROM kids
WHERE age IN (2, 4, 6, 8, 10);
```
* Make sure to always put the `ORDER BY` clause at the end of your query. You can't sort values that you haven't calculated yet!

---
### Filtering results

Congrats on finishing the first chapter! You now know how to select columns and perform basic counts. This chapter will focus on filtering your results.

In SQL, the  `WHERE`  keyword allows you to filter based on both text and numeric values in a table. There are a few different comparison operators you can use:

-   `=`  equal
-   `<>`  not equal
-   `<`  less than
-   `>`  greater than
-   `<=`  less than or equal to
-   `>=`  greater than or equal to

For example, you can filter text records such as  `title`. The following code returns all films with the title  `'Metropolis'`:

```
SELECT title
FROM films
WHERE title = 'Metropolis';

```

Notice that the  `WHERE`  clause always comes after the  `FROM`  statement!

**Note that in this course we will use  `<>`  and not  `!=`  for the not equal operator, as per the SQL standard.**

---
### Introduction to NULL and IS NULL

In SQL,  `NULL`  represents a missing or unknown value. You can check for  `NULL`  values using the expression  `IS NULL`. For example, to count the number of missing birth dates in the  `people`  table:

```
SELECT COUNT(*)
FROM people
WHERE birthdate IS NULL;

```

As you can see,  `IS NULL`  is useful when combined with  `WHERE`  to figure out what data you're missing.

Sometimes, you'll want to filter out missing values so you only get results which are not  `NULL`. To do this, you can use the  `IS NOT NULL`  operator.

For example, this query gives the names of all people whose birth dates are  _not_  missing in the  `people`  table.

```
SELECT name
FROM people
WHERE birthdate IS NOT NULL;

```
---
### LIKE and NOT LIKE

As you've seen, the  `WHERE`  clause can be used to filter text data. However, so far you've only been able to filter by specifying the exact text you're interested in. In the real world, often you'll want to search for a  _pattern_  rather than a specific text string.

In SQL, the  `LIKE`  operator can be used in a  `WHERE`  clause to search for a pattern in a column. To accomplish this, you use something called a  _wildcard_  as a placeholder for some other values. There are two wildcards you can use with  `LIKE`:

The  `%`  wildcard will match zero, one, or many characters in text. For example, the following query matches companies like  `'Data'`,  `'DataC'`  `'DataCamp'`,  `'DataMind'`, and so on:

```
SELECT name
FROM companies
WHERE name LIKE 'Data%';

```

The  `_`  wildcard will match a  _single_  character. For example, the following query matches companies like  `'DataCamp'`,  `'DataComp'`, and so on:

```
SELECT name
FROM companies
WHERE name LIKE 'DataC_mp';

```

You can also use the  `NOT LIKE`  operator to find records that  _don't_  match the pattern you specify.

---
### Aggregate functions

Often, you will want to perform some calculation on the data in a database. SQL provides a few functions, called  _aggregate functions_, to help you out with this.

For example,

```
SELECT AVG(budget)
FROM films;

```

gives you the average value from the  `budget`  column of the  `films`  table. Similarly, the  `MAX`  function returns the highest budget:

```
SELECT MAX(budget)
FROM films;

```

The  `SUM`  function returns the result of adding up the numeric values in a column:

```
SELECT SUM(budget)
FROM films;

```

You can probably guess what the  `MIN`  function does! Now it's your turn to try out some SQL functions.

--- 
### A note on arithmetic

In addition to using aggregate functions, you can perform basic arithmetic with symbols like  `+`,  `-`,  `*`, and  `/`.

So, for example, this gives a result of  `12`:

```
SELECT (4 * 3);

```

However, the following gives a result of  `1`:

```
SELECT (4 / 3);

```

What's going on here?

SQL assumes that if you divide an integer by an integer, you want to get an integer back. So be careful when dividing!

If you want more precision when dividing, you can add decimal places to your numbers. For example,

```
SELECT (4.0 / 3.0) AS result;

```

gives you the result you would expect:  `1.333`.

---
### It's AS simple AS aliasing

You may have noticed in the first exercise of this chapter that the column name of your result was just the name of the function you used. For example,

```
SELECT MAX(budget)
FROM films;

```

gives you a result with one column, named  `max`. But what if you use two functions like this?

```
SELECT MAX(budget), MAX(duration)
FROM films;

```

Well, then you'd have two columns named  `max`, which isn't very useful!

To avoid situations like this, SQL allows you to do something called  _aliasing_. Aliasing simply means you assign a temporary name to something. To alias, you use the  `AS`  keyword, which you've already seen earlier in this course.

For example, in the above example we could use aliases to make the result clearer:

```
SELECT MAX(budget) AS max_budget,
       MAX(duration) AS max_duration
FROM films;

```

Aliases are helpful for making results more readable!

---
### ORDER BY

Congratulations on making it this far! You now know how to select and filter your results.

In this chapter you'll learn how to sort and group your results to gain further insight. Let's go!

In SQL, the  `ORDER BY`  keyword is used to sort results in ascending or descending order according to the values of one or more columns.

By default  `ORDER BY`  will sort in ascending order. If you want to sort the results in descending order, you can use the  `DESC`  keyword. For example,

```
SELECT title
FROM films
ORDER BY release_year DESC;

```

gives you the titles of films sorted by release year, from newest to oldest.

**NOTE** : 
`ORDER BY`  can also be used to sort on multiple columns. It will sort by the first column specified, then sort by the next, then the next, and so on. For example,

```
SELECT birthdate, name
FROM people
ORDER BY birthdate, name;

```

sorts on birth dates first (oldest to newest) and then sorts on the names in alphabetical order.  **The order of columns is important!**

---
### GROUP BY

Now you know how to sort results! Often you'll need to aggregate results. For example, you might want to count the number of male and female employees in your company. Here, what you want is to group all the males together and count them, and group all the females together and count them. In SQL,  `GROUP BY`  allows you to group a result by one or more columns, like so:

```
SELECT sex, count(*)
FROM employees
GROUP BY sex;

```

This might give, for example:
|Sex| count |
|--|--|
| male |15|
|female|19|

Commonly,  `GROUP BY`  is used with  _aggregate functions_  like  `COUNT()`  or  `MAX()`. Note that  `GROUP BY`  always goes after the  `FROM`  clause!

**A word of warning:** 
 SQL will return an _error_ if you try to  `SELECT`  a field that is not in your  `GROUP BY`  clause without using it to calculate some kind of value about the entire group.

Note that you can combine  `GROUP BY`  with  `ORDER BY`  to group your results, calculate something about them, and then order your results. For example,

```
SELECT sex, count(*)
FROM employees
GROUP BY sex
ORDER BY count DESC;
```
---
### HAVING a great time

In SQL, aggregate functions can't be used in  `WHERE`  clauses. For example, the following query is invalid:

```
SELECT release_year
FROM films
GROUP BY release_year
WHERE COUNT(title) > 10;

```

This means that if you want to filter based on the result of an aggregate function, you need another way! That's where the  `HAVING`  clause comes in. For example,

```
SELECT release_year
FROM films
GROUP BY release_year
HAVING COUNT(title) > 10;

```

shows only those years in which more than 10 films were released.

---
