# Portfolio
# Netflix Analysis Using SQL, Tableau, and Excel
Netflix analysis
Excel Cleaning
1. =IF(D2=""; "N/A"; D2) --replace null values with "N/A" for 'Director' column
2. =SUBSTITUTE(D8; "Ã©"; "e") --normalize incorrect characters on 'Director' column
3. =PROPER(C2) --make sure all values are properly written with capital and non-capital letter in 'title'
4. Deleted 'Cast' and 'Description' columns as they are not needed for visualization and analysis
5. Deleted blank rows from 'rating' and 'duration' --total 7 rows won't affect much on the data
6. Data -> Split Text to Columns -> comma --to include one country per row only
7. =TEXT(F2; "YYYY-MM-DD") --change format of 'date_added'

SQL Cleaning
1. --The show_id column is the unique id for the dataset, therefore we are going to check for duplicates

SELECT show_id, COUNT(*)                                                                                                                                                                            
FROM Netflix_analysis
GROUP BY show_id                                                                                                                                                                                            
HAVING COUNT(*)>1

--No duplicates

2. --Check nulls on country
SELECT Count(*)
FROM Netflix_analysis
WHERE Country IS NULL


3. --Fill null country that has same director
UPDATE nt
SET nt.country = nt2.country
FROM Netflix_analysis AS nt
JOIN Netflix_analysis AS nt2
  ON nt.director = nt2.director
  AND nt.show_id <> nt2.show_id
WHERE nt.country IS NULL;

4. --To confirm if there are still directors linked to country that refuse to update

SELECT director, country, date_added
FROM Netflix_analysis
WHERE country IS NULL;

5. --Populate the rest of the NULL in director as "Not Given"

UPDATE Netflix_analysis 
SET country = 'N/A'
WHERE country IS NULL;

--KPI: Percentage of Movies vs. TV Shows
SELECT 
    CAST((COUNT(CASE WHEN type = 'Movie' THEN 1 END) * 100.0) / COUNT(*)AS decimal (10,2)) AS Percentage_movies,
    CAST((COUNT(CASE WHEN type = 'TV Show' THEN 1 END) * 100.0) / COUNT(*)AS decimal (10,2)) AS Percentage_tv_shows
FROM Netflix_analysis;

--KPI: Number of Movies/TV Shows by Country
SELECT	Country,
	COALESCE(SUM(CASE WHEN type = 'Movie' THEN 1 END),0) AS Movies,
	COALESCE(SUM(CASE WHEN type = 'TV Show' THEN 1 END),0) AS TV_Shows
FROM Netflix_analysis
GROUP BY Country

--KPI: Count of Movies/TV Shows by Rating
SELECT	Rating,
	COUNT(type) AS Type
FROM Netflix_analysis
GROUP BY Rating
ORDER BY COUNT(type) DESC

--KPI: Number of Movies/TV Shows Released by Year
SELECT TOP 10 Release_year,
	COUNT(type) Type
FROM Netflix_analysis
GROUP BY Release_year
ORDER BY Release_year DESC

--KPI: Number of Movies/TV Shows Added by Date
SELECT YEAR(Date_added) AS Date_added,
	COALESCE(COUNT(CASE WHEN type = 'Movie' THEN 1 END),0) AS Movies,
	COALESCE(COUNT(CASE WHEN type = 'TV Show' THEN 1 END),0) AS TV_Shows
FROM Netflix_analysis
WHERE Year(Date_added) IS NOT NULL
GROUP BY YEAR(Date_added)
ORDER BY Year(Date_added) DESC

--KPI: Number of Movies/TV Shows by Director
SELECT TOP 10 Director,
	COALESCE(COUNT(CASE WHEN type = 'Movie' THEN 1 END),0) AS Movies,
	COALESCE(COUNT(CASE WHEN type = 'TV Show' THEN 1 END),0) AS TV_Shows
FROM Netflix_analysis
WHERE Director != 'N/A'
GROUP BY Director
ORDER BY COALESCE(COUNT(CASE WHEN type = 'Movie' THEN 1 END),0) DESC, COALESCE(COUNT(CASE WHEN type = 'TV Show' THEN 1 END),0) DESC

--KPI: Top Genres on Netflix
SELECT TOP 10 Listed_in as Genre, COUNT(*) AS Total
FROM Netflix_analysis
GROUP BY Listed_in
ORDER BY COUNT(*) DESC

Tableau Data Visualization
![image](https://github.com/user-attachments/assets/e61b455a-6e1e-4c8a-969d-c1377b5aead0)
