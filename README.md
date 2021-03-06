# sql_training
practicing sql  

sql lessons on sqlbolt and exercise

-- CH6 - Multi-table queries with JOINs

-- Find the domestic and international sales for each movie
SELECT Title, International_sales, Domestic_sales
FROM Movies JOIN Boxoffice
ON Id=Movie_id;

-- Show the sales numbers for each movie that did better internationally rather than domestically
SELECT Title, International_sales, Domestic_sales
FROM Movies JOIN Boxoffice
ON Id=Movie_id
WHERE International_sales > Domestic_sales;

-- List all the movies by their ratings in descending order
SELECT Title, Rating
FROM Movies JOIN Boxoffice
ON Id=Movie_id
ORDER BY Rating DESC;

-- CH7 - OUTER JOIN

-- Find the list of all buildings that have employees
SELECT DISTINCT Building
FROM Employees
LEFT JOIN Buildings ON Building=Building_name
WHERE Years_employed NOT NULL;

-- Find the list of all buildings and their capacity
SELECT *
FROM Buildings;

-- List all buildings and the distinct employee roles in each building (including empty buildings)
SELECT DISTINCT Building_name, Role 
FROM Buildings 
LEFT JOIN employees ON building_name = building;

-- CH8 - A short note on NULLs

-- Find the name and role of all employees who have not been assigned to a building
SELECT *
FROM Employees
LEFT JOIN Buildings
ON Building_name = Building
WHERE Building IS NULL;

-- Find the names of the buildings that hold no employees
SELECT *
FROM Buildings
LEFT JOIN Employees
ON Building_name = Building
WHERE Building IS NULL;

-- CH9 - Queries with expressions

-- List all movies and their combined sales in millions of dollars
SELECT Title, (Domestic_sales + International_sales)/1000000 AS Total_Sales_Millions
FROM Movies
LEFT JOIN Boxoffice ON Id=Movie_Id;

-- List all movies and their ratings in percent
SELECT Title, Rating*10 as Percent
FROM Movies
LEFT JOIN Boxoffice ON Id=Movie_Id;

-- List all movies that were released on even number years
SELECT Title, Year
FROM Movies
LEFT JOIN Boxoffice ON Id=Movie_Id
WHERE Year % 2 = 0;

-- CH10 - Queries with aggregates (Pt. 1)

-- Find the longest time that an employee has been at the studio
SELECT MAX(Years_employed)
FROM Employees;

-- For each role, find the average number of years employed by employees in that role
SELECT Role, AVG(Years_Employed) 
FROM Employees
GROUP BY Role;

-- Find the total number of employee years worked in each building
SELECT Building, SUM(Years_Employed) 
FROM Employees
GROUP BY Building;

-- CH11 - Queries with aggregates (Pt. 2)

-- Find the number of Artists in the studio (without a HAVING clause)
SELECT Role, COUNT(*) AS Number_of_Artists
FROM Employees
WHERE Role = "Artist";

-- Find the number of Employees of each role in the studio
SELECT Role, COUNT(*)
FROM Employees
GROUP BY Role;

-- Find the total number of years employed by all Engineers
SELECT Role, SUM(Years_Employed)
FROM Employees
GROUP BY Role
HAVING Role = "Engineer";

-- CH12 - Order of execution of a Query

-- Find the number of movies each director has directed
SELECT *, COUNT(Title)
FROM Movies
GROUP BY Director;

-- Find the total domestic and international sales that can be attributed to each director
SELECT Director, sum(Domestic_sales + International_Sales) as Total_Sales
FROM Movies
LEFT JOIN Boxoffice ON Id = Movie_ID
GROUP BY Director;

-- CH13 - Inserting rows

-- Add the studio's new production, Toy Story 4 to the list of movies (you can use any director)
INSERT INTO Movies,
VALUES (4, "Toy Story 4", "John Lasseter", 2017, 123);

-- Toy Story 4 has been released to critical acclaim! It had a rating of 8.7, and made 340 million domestically and 270 million internationally. Add the record to the  BoxOffice table. 
INSERT INTO Boxoffice
VALUES (4, 8.7, 340000000, 270000000);

-- CH14 - Updating rows

-- The director for A Bug's Life is incorrect, it was actually directed by John Lasseter
UPDATE Movies
SET Director = "John Lasseter"
WHERE Id = 2;

-- The year that Toy Story 2 was released is incorrect, it was actually released in 1999
UPDATE Movies
SET Year = "1999"
WHERE Id = 3;

-- Both the title and directory for Toy Story 8 is incorrect! The title should be "Toy Story 3" and it was directed by Lee Unkrich
UPDATE Movies
SET Title = "Toy Story 3", Director = "Lee Unkrich"
WHERE Id = 11;

-- CH15 - Deleting rows

-- This database is getting too big, lets remove all movies that were released before 2005.
DELETE FROM Movies
WHERE Year < 2005;

-- Andrew Stanton has also left the studio, so please remove all movies directed by him.
DELETE FROM Movies
WHERE Director = "Andrew Stanton";

-- CH16 - Creating Tables

-- Create a new table named Database with the following columns:
-- 1. Name A string (text) describing the name of the database
-- 2. Version A number (floating point) of the latest version of this database
-- 3. Download_count An integer count of the number of times this database was downloaded
-- This table has no constraints.
CREATE TABLE Database (
    Name TEXT,
    Version FLOAT,
    Download_Count INTEGER);
    
-- CH17 - Altering Tables

-- Add a column named Aspect_ratio with a FLOAT data type to store the aspect-ratio each movie was released in.
ALTER TABLE Movies
  ADD COLUMN Aspect_ratio FLOAT DEFAULT 3;
  
-- Add another column named Language with a TEXT data type to store the language that the movie was released in. Ensure that the default for this language is English.
ALTER TABLE Movies
  ADD COLUMN Language TEXT DEFAULT "English";

-- CH18 - Dropping Tables

-- We've sadly reached the end of our lessons, lets clean up by removing the Movies table
DROP TABLE Movies;

-- And drop the BoxOffice table as well
DROP TABLE BoxOffice;
