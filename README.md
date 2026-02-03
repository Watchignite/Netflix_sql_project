# Netflix Movies and TV shows Data Analysis using SQL
![Netflix logo](https://github.com/Watchignite/Netflix_sql_project/blob/main/logo.png)
## Overview 
This project involves a comprehensive analysis of Netflix's Movies and TV Shows dataset using SQL. The goal is to extract meaningful insights, answer business questions, and showcase SQL capabilities required for data analyst roles.The following README provides a detailed account of the project's objectives, business problems, solutions, findings, and conclusions.
## Objective
- ✔️ Analyze the distribution of **Movies vs TV Shows**  
- ✔️ Identify the **most common content ratings**  
- ✔️ Retrieve content based on **year, genre, country, or director**  
- ✔️ Use **advanced SQL** for insight extraction  
- ✔️ Categorize content using **keyword-based filtering**  
- ✔️ Understand **geographical** and **historical** distribution patterns  
## 📂 Dataset

The datasets used in this project are sourced from **Kaggle**:

- **Netflix Movies & TV Shows Dataset**  
  🔗 *https://www.kaggle.com/datasets/shivamb/netflix-shows*

 Dataset include detailed information such as title, genre, rating, release year, cast, and more — enabling strong SQL-based analysis.
## Schema
``` sql
DROP TABLE IF EXISTS netflix;
CREATE TABLE netflix
(
    show_id      VARCHAR(5),
    type         VARCHAR(10),
    title        VARCHAR(250),
    director     VARCHAR(550),
    casts        VARCHAR(1050),
    country      VARCHAR(550),
    date_added   VARCHAR(55),
    release_year INT,
    rating       VARCHAR(15),
    duration     VARCHAR(15),
    listed_in    VARCHAR(250),
    description  VARCHAR(550)
);
```
## 🧠 Business Problems and SQL Solutions
### 1️⃣ Count the Number of Movies vs TV Shows
```sql
SELECT 
    type,
    COUNT(*)
FROM netflix
GROUP BY 1;
```
#### Objective: Determine the distribution of content types on Netflix.
### 2️⃣ Most Common Rating for Movies and TV Shows
```sql
WITH RatingCounts AS (
    SELECT 
        type,
        rating,
        COUNT(*) AS rating_count
    FROM netflix
    GROUP BY type, rating
),
RankedRatings AS (
    SELECT 
        type,
        rating,
        rating_count,
        RANK() OVER (PARTITION BY type ORDER BY rating_count DESC) AS rank
    FROM RatingCounts
)
SELECT 
    type,
    rating AS most_frequent_rating
FROM RankedRatings
WHERE rank = 1;
```
#### Objective: Identify the most frequently occurring rating for each type of content.
### 3️⃣ All Movies Released in a Specific Year (Example: 2020)
```sql
SELECT * 
FROM netflix
WHERE release_year = 2020;
```
#### Objective: Retrieve all movies released in a specific year.
### 4️⃣ Top 5 Countries with the Most Netflix Content
```sql
SELECT * 
FROM
(
    SELECT 
        UNNEST(STRING_TO_ARRAY(country, ',')) AS country,
        COUNT(*) AS total_content
    FROM netflix
    GROUP BY 1
) AS t1
WHERE country IS NOT NULL
ORDER BY total_content DESC
LIMIT 5;
```
#### Objective: Identify the top 5 countries with the highest number of content items.
### 5️⃣ Longest Movie
```sql
SELECT 
    *
FROM netflix
WHERE type = 'Movie'
ORDER BY SPLIT_PART(duration, ' ', 1)::INT DESC;
```
#### Objective: Find the movie with the longest duration.
###
