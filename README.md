# Countries_of_the_World_SQL_Project
Analyzed the 'Countries of the World' dataset using SQL to explore global metrics such as population, GDP, and life expectancy. Identified key trends, such as economic disparities and correlations between GDP and life expectancy. The project provided insights into population distribution and economic factors across countries.
Objective
The goal of this project was to perform a comprehensive data analysis on the 'Countries of the World' dataset to extract meaningful insights and trends about various global metrics. The analysis aimed to provide a detailed understanding of country-specific attributes such as population, area, GDP, and other socio-economic indicators.

Dataset Overview
Source: Kaggle
Dataset Name: 'Countries of the World'
Description: The dataset contains information about countries around the world, including various indicators such as population, area, GDP, and other economic and demographic statistics.
Key Data Columns
Country: Name of the country.
Population: Total population of the country.
Area (sq. km): Total land area of the country in square kilometers.
GDP: Gross Domestic Product in USD.
Continent: Continent where the country is located.
Language: Primary language spoken in the country.
Region: Geographic or political region of the country.
Life Expectancy: Average life expectancy of citizens.
Methodology
Data Import and Cleaning:

Imported the dataset into a SQL database.
Performed data cleaning tasks such as handling missing values, correcting data types, and ensuring consistency.
Exploratory Data Analysis (EDA):

Analyzed basic statistics of the dataset (e.g., mean, median, standard deviation).
Conducted exploratory queries to understand data distributions and identify any anomalies.
SQL Queries and Analysis:

Population Analysis: Identified the top 10 most populous countries and analyzed population density.
Area Analysis: Compared the land area of countries within different continents.
GDP Analysis: Evaluated GDP figures to determine the top economies and analyzed GDP per capita.
Life Expectancy: Investigated the relationship between life expectancy and GDP.
Advanced SQL Techniques:

Utilized SQL JOINs to merge data with external datasets if applicable.
Applied SQL functions and aggregations to summarize data and perform complex queries.
Created views and temporary tables for efficient querying and reporting.
Visualization and Reporting:

Created SQL-based reports and visualizations to present findings (e.g., using SQL-based tools or exporting data for use in external visualization tools).
Key Findings
Population Insights: The most populous countries were identified, with a notable concentration of high populations in certain regions.
Economic Disparities: Significant differences in GDP and GDP per capita were observed between continents and regions.
Geographic Patterns: There was a correlation between land area and economic indicators, with larger countries often having higher GDPs.
Life Expectancy Trends: Higher GDP countries generally exhibited higher life expectancy, highlighting the link between economic prosperity and quality of life.
Conclusions
The analysis provided valuable insights into global trends and disparities among countries. It revealed patterns in population distribution, economic strength, and socio-economic development. The findings underscore the impact of economic factors on life expectancy and highlight significant geographical variations.
#Code
Use countries_1;

SELECT * FROM `countries of the world`;

describe `countries of the world`;

-- Count Rows:
SELECT COUNT(*) FROM `countries of the world` ;

 -- DATA CLEANING
 -- 1) Decimal cleaning

 select Country,`Coastline (coast/area ratio)`,`Literacy (%)`,`Pop. Density (per sq. mi.)`,`Arable (%)`,`Crops (%)`,Service FROM `countries of the world`;
 set sql_safe_updates=0;
 
update `countries of the world`
set `Pop. Density (per sq. mi.)`=replace(`Pop. Density (per sq. mi.)`,',','.'),
`Coastline (coast/area ratio)`=replace(`Coastline (coast/area ratio)`,',','.'),
`Net migration`=replace(`Net migration`,',','.'),
`Infant mortality (per 1000 births)`=replace(`Infant mortality (per 1000 births)`,',','.'),
`Literacy (%)`=replace(`Literacy (%)`,',','.'),
`Phones (per 1000)`=replace(`Phones (per 1000)`,',','.'),
`Arable (%)`=replace(`Arable (%)`,',','.'),
`Crops (%)`=replace(`Crops (%)`,',','.'),
`Other (%)`=replace(`Other (%)`,',','.'),
Climate=replace(Climate,',','.'),
Birthrate=replace(Birthrate,',','.'),
Deathrate=replace(Deathrate,',','.'),
Agriculture=replace(Agriculture,',','.'),
Industry=replace(Industry,',','.'),
Service=replace(Service,',','.');


-- Handling Missing Values/empty string:

UPDATE `countries of the world` 
SET `Pop. Density (per sq. mi.)`=nullif(`Pop. Density (per sq. mi.)`,''),
`Coastline (coast/area ratio)`=nullif(`Coastline (coast/area ratio)`,''),
`Net migration`=nullif(`Net migration`,''),
`Infant mortality (per 1000 births)`=nullif(`Infant mortality (per 1000 births)`,''),
`Literacy (%)`=nullif(`Literacy (%)`,''),
`Phones (per 1000)`=nullif(`Phones (per 1000)`,''),
`Arable (%)`=nullif(`Arable (%)`,''),
`Crops (%)`=nullif(`Crops (%)`,''),
`Other (%)`=nullif(`Other (%)`,''),
Climate=nullif(Climate,''),
Birthrate=nullif(Birthrate,''),
Deathrate=nullif(Deathrate,''),
Agriculture=nullif(Agriculture,''),
Industry=nullif(Industry,''),
Service=nullif(Service,'');

-- convert datatypes

ALTER TABLE `countries of the world`
MODIFY COLUMN `Population` INT,
MODIFY COLUMN `Area (sq. mi.)` INT,
MODIFY COLUMN `Pop. Density (per sq. mi.)`  FLOAT,
MODIFY COLUMN `Coastline (coast/area ratio)` FLOAT,
MODIFY COLUMN `Net migration` FLOAT,
MODIFY COLUMN `Infant mortality (per 1000 births)` FLOAT,
MODIFY COLUMN `GDP ($ per capita)` FLOAT,
MODIFY COLUMN `Literacy (%)` FLOAT,
MODIFY COLUMN `Phones (per 1000)` FLOAT,
MODIFY COLUMN `Arable (%)` FLOAT,
MODIFY COLUMN `Crops (%)` FLOAT,
MODIFY COLUMN `Other (%)` FLOAT,
MODIFY COLUMN Climate INT,
MODIFY COLUMN Birthrate FLOAT,
MODIFY COLUMN Deathrate FLOAT,
MODIFY COLUMN Agriculture FLOAT,
MODIFY COLUMN Industry FLOAT,
MODIFY COLUMN  Service FLOAT;
SELECT * FROM `countries of the world`;
-- Feature Engineering:

-- 1)Add a column for GDP per capita:

 ALTER TABLE `countries of the world`
 ADD COLUMN GDP_per_Capita FLOAT;

UPDATE `countries of the world`
SET GDP_per_Capita = `GDP ($ per capita)` / Population;

select Country,`GDP ($ per capita)`, GDP_per_Capita from `countries of the world`;

-- 2)Create a categorical column for population density:

ALTER TABLE `countries of the world`
ADD COLUMN Density_Category VARCHAR(50);

UPDATE `countries of the world`
SET Density_Category = CASE
    WHEN `Pop. Density (per sq. mi.)` < 50 THEN 'Low'
    WHEN `Pop. Density (per sq. mi.)` BETWEEN 50 AND 200 THEN 'Medium'
    ELSE 'High'
END;
select Country, Density_Category from `countries of the world`;

-- 3)Extract the continent from the region:

ALTER TABLE `countries of the world`
ADD COLUMN Continent VARCHAR(50);

UPDATE `countries of the world`
SET Continent = CASE
    WHEN Region LIKE '%Asia%' or Region LIKE '%Near East%' THEN 'Asia'
    WHEN Region LIKE '%Africa%' THEN 'Africa'
    WHEN Region LIKE '%Europe%' THEN 'Europe'
    WHEN Region LIKE '%Americas%' THEN 'North America'
    WHEN Region LIKE '%Latin Amer. & Carib%' THEN 'South America'
    WHEN Region LIKE '%Oceania%' THEN 'Oceania'
    ELSE 'Other'
END;
select Country, Region,Continent from `countries of the world`;


-- 4) Climate

ALTER TABLE `countries of the world`
ADD COLUMN Climate_category VARCHAR(50);

UPDATE `countries of the world`
SET 
    Climate_category = CASE
        WHEN Climate = 1 THEN 'Tropical'
        WHEN Climate = 2 THEN 'Arid'
        WHEN Climate = 3 THEN 'Temperate'
        WHEN Climate = 4 THEN 'Cold'
        ELSE 'Unknown'
    END;
 -- 1.Which countries have the highest and lowest populations?
 SELECT Country, Population
FROM `countries of the world`
ORDER BY Population DESC
LIMIT 1;
 -- China 	1313973713
 
 SELECT Country, Population
FROM `countries of the world`
ORDER BY Population ASC
LIMIT 1;
 -- St Pierre & Miquelon 	7026
 
-- 2.What are the largest and smallest countries by land area?
 SELECT Country, `Area (sq. mi.)`
FROM `countries of the world`
ORDER BY `Area (sq. mi.)` DESC
LIMIT 1;
 -- Russia 	17075200
 SELECT Country, `Area (sq. mi.)`
FROM `countries of the world`
ORDER BY `Area (sq. mi.)` DESC
LIMIT 1;
-- Monaco 	2

-- 3.What are the top 10 countries by GDP and which country has the maximum GDP growth ?
 SELECT Country, `GDP ($ per capita)`
FROM `countries of the world`
ORDER BY `GDP ($ per capita)` DESC
LIMIT 10;

-- 4.What is the average population density across all countries?
SELECT AVG(`Pop. Density (per sq. mi.)`)
FROM `countries of the world`;
-- 380.71991150442443

-- 5.Which country is having the highest infant mortality rate?
 SELECT Country, `Infant mortality (per 1000 births)`
FROM `countries of the world`
ORDER BY `Infant mortality (per 1000 births)` DESC
LIMIT 1;
-- Angola 	191.19

-- 6.What is the average literacy rate by region?
SELECT Region, AVG(`Literacy (%)`) AS average_literacy
FROM `countries of the world`
GROUP BY Region
ORDER BY average_literacy DESC;

-- 7.Which country has the lowest rates of mobile phone usage?
-- UPDATE countries_world SET Phones (per 1000) = NULLIF(Phones (per 1000), '') WHERE Phones (per 1000) = '';

SELECT Country, `Phones (per 1000)`
FROM `countries of the world`
where `Phones (per 1000)` is not null
ORDER BY `Phones (per 1000)`
 limit 1;
-- Congo, Dem. Rep. 	0.2

-- 8.What is the birth and Death rate for each Continents, and find the continent with the highest birth and Death rate.
SELECT Continent,
AVG(Birthrate) AS avg_birth_rate,
AVG(Deathrate) AS avg_death_rate
FROM `countries of the world`
GROUP BY Continent;
-- 9.Literacy rate varies across different Continents? 
SELECT Continent, AVG(`Literacy (%)`) AS avg_literacy_rate
FROM `countries of the world`
GROUP BY Continent order by avg_literacy_rate desc;

-- 10.How many countries are included in the dataset?
SELECT COUNT(DISTINCT Country) AS total_countries
FROM  `countries of the world`;
-- 226

 
-- 11.Total Population by Region
SELECT Region, SUM(Population) AS Total_population
FROM `countries of the world`
GROUP BY Region
ORDER BY Total_population DESC;


-- 12.Population Density Analysis 
SELECT Country,Population,`Area (sq. mi.)`,
Population / `Area (sq. mi.)` AS population_density
FROM `countries of the world`
ORDER BY population_density DESC
LIMIT 10;


-- 13. When considering the coastline, find the countries with the highest coastlines. 
SELECT 
Country, `Coastline (coast/area ratio)`
FROM `countries of the world`
ORDER BY `Coastline (coast/area ratio)` DESC
LIMIT 1; 
-- Micronesia, Fed. St. 	870.66


-- 14. Find the countries with the most and least migrants.
SELECT Country, `Net migration`
FROM `countries of the world`
ORDER BY `Net migration` DESC
LIMIT 1;
-- Afghanistan 	23.06

SELECT Country, `Net migration`
FROM `countries of the world`
WHERE `Net migration` IS NOT NULL
ORDER BY `Net migration`
LIMIT 1;
-- Micronesia, Fed. St. 	-20.99

-- 15. Find the continent with the most and least migrants.
SELECT Continent, SUM(`Net migration`) AS total_migrants
FROM `countries of the world`
GROUP BY Continent
ORDER BY total_migrants DESC
LIMIT 1;
-- Europe	85.2299998998642
SELECT Continent, SUM(`Net migration`) AS total_migrants
FROM `countries of the world`
GROUP BY Continent
ORDER BY total_migrants ASC
LIMIT 1;
-- South America	-67.24999941326678

-- 16) Average GDP per Capita by Continent

SELECT Continent, AVG(GDP_per_Capita) AS avg_GDP_per_Capita
FROM countries of the world
GROUP BY Continent
ORDER BY avg_GDP_per_Capita DESC;


-- 17) Highest GDP per Capita in Each Continent
SELECT Continent,Country, GDP_per_Capita
FROM `countries of the world`
WHERE GDP_per_Capita = (
    SELECT MAX(GDP_per_Capita)
    FROM `countries of the world` c2
    WHERE c2.Continent = `countries of the world`.Continent
);

-- 19)Distribution of Density Categories
SELECT Density_Category, COUNT(*) AS count
FROM `countries of the world`
GROUP BY Density_Category;

SELECT Continent, Climate_category, COUNT(*) AS Number_of_Countries
FROM `countries of the world`
GROUP BY Continent, Climate_category
ORDER BY Continent, Climate_category;
