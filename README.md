# SQL-Personal-Tutorial

This is where I will be uploading my notes on SQL from beginner to advanced level. 
If you see any mistakes or anything that needs corrections please reach out and let me know. 
# Introduction
SQL stands for Structured Query Language. SQL is a query language mainly used to manage data which means that it is used for accessing and manipulating databases. SQL has many different versions for example: MySQL, PostgreSQL, MS SQL Server, Oracle, and many more. This is due to SQL just being a language and this language needs an engine to run it which is where the different types of SQL come in due to how each engine interprets SQL differently. Even though there are many different forms of SQL they all follow the same format in which they all contain similar commands (e.g. SELECT, FROM, UPDATE, WHERE, GROUP BY, and etc). At the end of the day if you know one version of SQL, don’t panic and think that you will take for ever to learn a new language the next day, they are all very similar to each other with only small minor differences making transitions between the different SQL very smooth. 

In this tutorial, I will mainly use MS SQL but I will mention other SQL languages in case of any discrepancies. 

# Beginner
# SELECT + FROM
In SQL, the SELECT statement is used to select data from a database.<br/>

SELECT Column1, Column2, ...<br/>
FROM DATABASE;

The column1 and column2 are the column names that you want to select and DATABASE represents the table that you are selecting FROM. 

If you want to select all of the columns it is doable by replacing all of the column names with an asterisk, *. <br/>

SELECT *<br/>
FROM  DATABASE;


Keep in mind that SQL is not case sensitive so that means that SQL will read both select and SELECT the same. 

While some languages may require a semicolon at the end of each query most of the time it is not necessary. The semi-colon just serves as a representation of where the query ends and when the next query starts. 

# LIMIT
The LIMIT statement shortens the amount of information shown. Most SQL operators don’t use LIMIT but use TOP instead but they all work the same way by showing the first few specified rows

SELECT TOP 5 Title <br/>
FROM Movies

Will return the first 5 movie titles from the movie dataset. And in other versions, you’ll see limit being used instead of TOP.

SELECT Title<br/>
FROM Movies<br/>
LIMIT 5 

Both codes will do the same thing by providing you with the first specified number of rows to you kind of like head() in Python. 

# DISTINCT
The distinct statement is used to only collect all of the distinct values. Sometimes there will be tables that have duplicate values and there will be times where you only want the list of distinct values. <br/>

SELECT DISTINCT Genres<br/>
FROM Movies

This query returns all of the different types of genres within the Genres column from the Movies table. Otherwise, if you omit the DISTINCT statement you will get all of the values from the Genres column. 

# Intermediate

# WHERE
The WHERE statement is used very frequently in SQL to filter for specific criteria. It helps retrieve only the rows that meet the given conditions. <br/>

SELECT Movies, Genres<br/>
FROM Movies <br/>
WHERE Genres = 'Action'

This query selects movies and their genres from the Movies table where the genre is "Action."
The WHERE clause supports many operators for filtering data:

" = "
Equal<br/>
" < "
Less than <br/>
" > "
Greater than <br/>
" <= "
Less than or equal to<br/>
" >= " 
Greater than or equal to <br/>
" <> "
Not equal to (Some other versions of SQL may use != instead)<br/>
BETWEEN
Between a certain range<br/>
LIKE
Used to find a specified pattern <br/>
IN
Used to match specified values in a list<br/>

The LIKE operator is used to search for a specified pattern within text values. It supports two wildcards:
- % represents zero, one, or multiple characters.
- _ represents a single character.<br/>

SELECT *<br/>
FROM Movies <br/>
WHERE Title like 'Star Wars%'

This query retrieves all movies where the title starts with "Star Wars."

SELECT *<br/>
FROM Movies <br/>
Where Title like 'St_r W_rs'

This query selects movies where the title follows the pattern "St", any single character, "r W", any single character, and "rs."

SQL WHERE clause also supports logical operators for more advanced filters. 

AND 
Both conditions must be true.

OR 
Either condition can be true or both can be true.

NOT
Returns the opposite of the given condition. 

When working with datasets, you may encounter NULL values, which represent missing or unknown data. You can filter for NULL values using IS NULL:

SELECT *<br/>
FROM Movies <br/>
WHERE Title IS NULL

This query will return all the rows where the title is null or blank. 

# Aggregate Functions
An aggregate function executes calculations based on the specified value. 

AVG() 
Returns the average value of the specified data<br/>
SUM() 
Returns the total sum of the data<br/>
MIN()
Returns the smallest value<br/>
MAX()
Returns the greatest value<br/>
COUNT()
Counts the number of rows in a set<br/>


Most of Aggregate functions will skip over null values except for COUNT() where it will include null valaues when counting.

Aggregate functions are typically used with the SELECT statement:

SELECT AVG(Ratings)<br/>
FROM Movies

This will provide the average Ratings for all movies. Same structure can be done for the other aggregate functions including SUM(), MAX(), MIN() and COUNT() to get their respective outputs

# Rounding
The round function will round a number to a specified number of decimal places:

ROUND(number, decimal place)

If the decimal place is positive then it will round after the decimal and if the decimal place number is positive then it will round before the decimal place. 
For example: <br/>
ROUND(123.456, 2) = 123.46<br/>
And<br/>
ROUND(123.456,-2) = 120.000

# ORDER BY
The ORDER BY statement is used to sort one or more fields. What is does is basically sort the data from smallest to largest by or alphabetically by default. If you want to sort it the reverse method then you have to add DESC after the column name.<br/>

SELECT Title, Ratings<br/>
FROM Movies<br/>
ORDER BY Ratings DESC

This query retrieves the Title and Ratings columns from the Movies dataset and sorts them from highest to lowest rating.

Again ORDER BY can sort through multiple fields where the second field acts as a tie-breaker when the first field has duplicate values:<br/>

SELECT Title, Ratings<br/>
FROM Movies<br/>
ORDER BY Title ASC , Ratings DESC

This query will select the title and ratings columns from the movies dataset and it will order by the title column alphabetically but if for some reason there are movies with the same title for example a series then it will order the series by rating from best to worst. 

# GROUP BY
The GROUP BY statement is a really important part of SQL. As the name states, the GROUP BY statement is used to group your data. What the group by groups are the values where it is the same. The GROUP BY statement is normally used along with aggregate functions to provide summary statistics. 

SELECT Genres, COUNT(genres)<br/>
FROM Movies<br/>
GROUP BY Genres

Here, the GROUP BY statement groups all the movies by their Genres and counts the number of movies in each genre.

# Having
The HAVING statement is more situational, but it is used when it is necessary. The HAVING statement filters through your aggregated data or groups, as the WHERE statement can’t. Once your data has been aggregated and grouped, the HAVING statement filters through each group. 

Sometimes it is also important to understand the order of execution to understand why we can’t use WHERE to filter through our grouped data.
With SQL the order of execution is not the order it is written.

1. FROM
2. WHERE (filters individual rows)
3. GROUP BY (groups the filtered data)
4. HAVING (filters groups)
5. SELECT
6. ORDER BY
7. LIMIT

Since WHERE is executed before GROUP BY, it cannot filter grouped data. Instead, HAVING is used after grouping to filter groups based on the conditions.

# Alias
Aliases improve readability and help avoid mistakes by renaming columns or tables in SQL queries. The AS keyword assigns a temporary name to a column or calculation.

SELECT Title, AVG(Ratings) as AvgRatings <br/>
FROM Movies<br/>
GROUP BY Title<br/>
HAVING AvgRatings >= 7<br/>
ORDER BY AvgRatings

The query assigns the average rating column a name AvgRating, filters for movies with average rating greater than 7, and sorts the results in descending order.