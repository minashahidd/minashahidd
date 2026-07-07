USE world;
SELECT * FROM city;
SELECT * FROM country;

# Select one column
SELECT CountryCode FROM city;

# Select multiple columns
SELECT CountryCode, District FROM city;

# Select with an alias
SELECT CountryCode AS "Country Code" FROM city;

# Complex query with aliases
SELECT 
    CountryCode AS 'Country Code',
    District,
    Name AS 'Country Name'
FROM
    city;

# Select 10 rows
SELECT *
FROM city
LIMIT 10; # Default = 1000

# Get the average population by city
SELECT
	AVG(Population) # Excel -> =AVERAGE(Population)
FROM 
	city;
    
# Full query example
SELECT
	*
FROM 
	city
#WHERE
	#Population > 2000
ORDER BY
	Population DESC # Default = ASC 
LIMIT
	10;
    
# Country with Highest Life Expectancy
SELECT 
    Name AS 'Country Name', 
    LifeExpectancy
FROM
    country
ORDER BY 
	LifeExpectancy DESC
LIMIT 1;

SELECT * FROM city ORDER BY Name;

SELECT * FROM city LIMIT 1;
SHOW CREATE TABLE city;

# Where clause with equal
SELECT * FROM city WHERE id = 2;
SELECT * FROM city WHERE CountryCode = "AFG";

# Where clause with mathematical operator
SELECT * FROM city WHERE id >= 5;
SELECT * FROM city WHERE Population > 10000 LIMIT 5000;
SELECT * FROM city WHERE Population > 10000;

# Where clause with number between two values
SELECT * FROM city WHERE Population BETWEEN 10000 AND 15000;

# Strings
# Full string match
SELECT * FROM city WHERE CountryCode = "AFG";

# Partial string match LIKE %
SELECT * FROM city WHERE CountryCode LIKE "A%"; # Starts with letter A
SELECT * FROM city WHERE CountryCode LIKE "AR%"; # Starts with AR
SELECT * FROM city WHERE CountryCode LIKE "%N"; # Finishes with N
SELECT * FROM city WHERE CountryCode LIKE "%Z%"; # Contains letter Z

# "2020-01-01 10:10:10" -> Timestamp
SELECT * FROM city WHERE YEAR(timestamp) = "2026";
SELECT * FROM city WHERE MONTH(timestamp) = "12";
SELECT * FROM city WHERE DAY(timestamp) = "02";
SELECT * FROM city WHERE HOUR(timestamp) = "11";

# Multiple conditions
SELECT * FROM city
WHERE YEAR(timestamp) = "2026" AND
	MONTH(timestamp) = "12" AND
    DAY(timestamp) = "02";

SELECT * FROM city
WHERE YEAR(timestamp) = "2026" OR
	MONTH(timestamp) = "12" OR
    DAY(timestamp) = "02";

SELECT * 
FROM city
WHERE Name LIKE '%New%';

# Returns the max(population) for when multiple cities have the same name
SELECT 
	Name, 
    MAX(Population) # Aggregation
FROM
	city
GROUP BY Name;

# Number of cities with the same name
SELECT
	Name, 
    COUNT(1)
FROM
	city
GROUP BY Name;
    
SELECT Name FROM city; # -> Returns > 1000 rows
SELECT MAX(Population) FROM city; # -> Returns 1 cell

SELECT * FROM city LIMIT 1;
SELECT * FROM country LIMIT 1;

SELECT *
FROM city
JOIN country ON country.Code = city.CountryCode;

SELECT *
FROM city
JOIN country ON  city.CountryCode = country.Code;

SELECT * 
FROM country
JOIN city ON country.Code = city.CountryCode;

SELECT * 
FROM country
JOIN city ON city.CountryCode = country.Code;

# Get all cities starting with the letter M in all countries
# starting with the letter M
SELECT * 
FROM country
JOIN city ON city.CountryCode = country.Code
WHERE country.Name LIKE "M%" AND
	city.Name LIKE "M%";

# Find all cities in the country called Spain
SELECT * 
FROM country
INNER JOIN city ON city.CountryCode = country.Code
WHERE country.Name = "Spain";

SELECT * FROM city WHERE CountryCode = "ESP";

USE world;
# 984 rows = 984 rows from Table 1 matched with Table 2
SELECT *
FROM countrylanguage
INNER JOIN country ON countrylanguage.CountryCode = country.Code;

# 984 rows = All rows from Table 1 with their match from Table 2
SELECT *
FROM countrylanguage
LEFT JOIN country ON countrylanguage.CountryCode = country.Code;

SELECT COUNT(1) FROM countrylanguage; # 984 rows

# 990 rows = All rows from Table 2 with their match from Table 1 adding Null when no match
SELECT *
FROM countrylanguage
RIGHT JOIN country ON countrylanguage.CountryCode = country.Code;

SELECT COUNT(1) FROM country; # 239 rows = more than one language by country

SELECT COUNT(1) FROM countrylanguage;


# Get average
SELECT AVG(Population) FROM city;

# Hardcode the average in the query (Not Good!)
SELECT * 
FROM city
WHERE Population > 350468.2236;

# Subquery to compare data with
SELECT * 
FROM city
WHERE Population > (SELECT AVG(Population) FROM city);

# Subquery in FROM statement
SELECT * 
FROM (
	SELECT * 
	FROM city
	WHERE 
		countryCode = "USA") as tmp
WHERE tmp.Population > 100000;

SELECT * 
FROM city
WHERE CountryCode IN ("AFG", "USA"); # -> WHERE CountryCode = "AFG" OR CountryCode = "USA

# Fiding country codes matching the WHERE Statement
SELECT Code
FROM country
WHERE Population BETWEEN 1000 and 2000;

# Harcoding the values in multiple WHERE statements
SELECT *
FROM city
WHERE 
	CountryCode = "FLK" OR 
    CountryCode = "NFK" OR 
    CountryCode = "NIU" OR 
    CountryCode = "TKL"OR 
    CountryCode = "VAT";

# Using IN instead of multiple where statements
SELECT *
FROM city
WHERE CountryCode IN ("FLK", "NFK", "NIU", "TKL", "VAT");

# Using a subquery instead of hardcoding the values
SELECT *
FROM city
WHERE CountryCode IN (
	SELECT Code
	FROM country
	WHERE Population BETWEEN 1000 and 2000
    )
;

# Get all unique values
SELECT DISTINCT(CountryCode)
FROM city;

# Get a count of unique values
SELECT DISTINCT(CountryCode), COUNT(1)
FROM city
GROUP BY CountryCode;

# Null values with filters
SELECT * FROM country
WHERE IndepYear = NULL; # Not working

SELECT * FROM country
WHERE IndepYear IS NULL; # Use IS with NULL

# Not equal to
SELECT * 
FROM countrylanguage
WHERE CountryCode NOT LIKE "CAN";

# Not equal to
SELECT * 
FROM countrylanguage
WHERE CountryCode != "CAN";

# Use of list in WHERE statement
SELECT country.Name, countrylanguage.* 
FROM countrylanguage
JOIN country ON countrylanguage.CountryCode = Country.Code
WHERE CountryCode IN ("CAN", "USA", "AUS");

# Integration process -> Python + MySQL Database
# Human error
# Humans just try their best
# Humans just do what they can
-- WHERE column IS NULL;
-- WHERE column = "NULL";
-- WHERE column = "Null";
-- WHERE column = "";
-- WHERE column = " ";
-- WHERE column = "Blank";


# Create a new Table
CREATE TABLE example (
	id INT NOT NULL AUTO_INCREMENT, 
    student_name VARCHAR(100) NOT NULL COMMENT "The name of the student", 
    student_age INT,
    PRIMARY KEY (id)
    );
# Data insertion
INSERT INTO example (student_name, student_age) 
VALUES ("Sebastien", "28"); # Transtyping of string to integer
INSERT INTO example (student_name, student_age)
VALUES ("Betty", 8);
INSERT INTO example (student_name, student_age)
VALUES ("Norbert", "twenty two"); # Raises Error
INSERT INTO example (student_name, student_age)
VALUES ("Norbert", 5); # Now works

# Delete Table
DROP TABLE example;

# Changing data
UPDATE example
SET student_name = "Jacques"
WHERE id = 1;

# Changing data without using primary key
UPDATE example
SET student_age = 12
WHERE student_name = "Jacques"; # Does not work in all environment

# Some function...
# MAX()
SELECT MAX(Population) 
FROM country; # -> Returns 1 cell

# AVG() average
SELECT AVG(Population)
FROM country; # -> Returns 1 cell

# CONCAT() concatenation
SELECT CONCAT(Name, " - Population: ", Population) # spaces need to be written manually
FROM country;

# DISTINCT() returns all unique values
SELECT DISTINCT(Language)
FROM countrylanguage;

# COUNT() number of values
SELECT COUNT(1)
FROM countrylanguage;

# Using multiple functions
SELECT DISTINCT(Language), COUNT(1)
FROM countrylanguage
GROUP BY Language;

