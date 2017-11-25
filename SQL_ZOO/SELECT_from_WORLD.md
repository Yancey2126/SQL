### SELECT from WORLD Tutorial (SQLZOO)

[TOC]

#### Country Profile
  In this tutorial you will use the SELECT command on the *table* world:

#### Introduction
  1. Read the notes about this table. Observe the result of running this SQL command to show the name, continent and population of all countries.
  ``` SQL
    SELECT name, continent, population FROM world
  ```
#### Large Countries
  2. Show the name for the countries that have a population of at least 200 million. 200 million is 200000000, there are eight zeros.
  ``` SQL
  SELECT name FROM world
  WHERE population >= 200000000
  ```
#### Per capita GDP
  3. Give the *name* and the *per capita GDP* for those countries with a *population* of at least 200 million.
  ``` SQL
  SELECT name, gdp/population FROM world
  WHERE population >= 200000000
  ```
#### South America In millions
  4. Show the *name* and *population* in millions for the countries of the *continent* 'South America'. Divide the population by 1000000 to get population in millions.
  ``` SQL
  SELECT name, population/1000000 FROM world
  WHERE continent = 'South America'
  ```
#### France, Germany, Italy
  5. Show the *name* and *population* for France, Germany, Italy
  ``` SQL
  SELECT name, population FROM world
  WHERE name in ('France', 'Germany', 'Italy')
  ```
#### United
  6. Show the countries which have a name that includes the word 'United'
  ``` SQL
  SELECT name FROM world
  WHERE name LIKE '%United%'
  ```
#### Two ways to be big
  7. Two ways to be big: A country is big if it has an area of more than 3 million sq km or it has a population of more than 250 million.
  Show the countries that are big by area **or** big by population. Show *name*, *population* and *area*.
  ``` SQL
  SELECT name, population, area FROM world
  WHERE population > 250000000 or area > 3000000
  ```
#### One or the other (but not both)r
  8. Exclusive OR (XOR). Show the countries that are big by area or big by population but not both. Show name, population and area.
      - Australia has a big area but a small population, it should be included. it should be included. it should be included. it should be included.
      - Indonesia has a big population but a small area, it should be included. it should be included. it should be included. it should be included.
      - China has a big population and big area, it should be excluded. be excluded. should be excluded. be excluded.
      - United Kingdom has a small population and a small area, it should be excluded. area, it should be excluded. small area, it should be excluded. area, it should be excluded.
  ``` SQL
  SELECT name, population, area FROM world
  WHERE population > 250000000 XOR area > 3000000
  ```
#### Rounding
  9. Show the name and population in millions and the GDP in billions for the countries of the continent 'South America'. Use the **ROUND** function to show the values to two decimal places.

      > ROUND(f,p) returns f rounded to p decimal places.

  ``` SQL
  SELECT name, round(population/1000000, 2), round( GDP/1000000000, 2)
  FROM world
  WHERE continent = 'South America'
  ```
#### Trillion dollar economies
  10. Show the *name* and *per-capita GDP* for those countries with a GDP of at least one trillion (1000000000000; that is 12 zeros). Round this value to the nearest 1000. **Show per-capita GDP for the trillion dollar countries to the nearest $1000.**

      > ROUND function can use negative argument to do reverse rounding

  ``` SQL
  SELECT name, round(GDP/population, -3) FROM world
  WHERE GDP >= 1000000000000
  ```

#### Name and capital have the same length
  11. Greece has capital Athens.
      Each of the strings 'Greece', and 'Athens' has 6 characters.
      **Show the name and capital where the name and the capital have the same number of characters.**

      > LENGTH(s) returns the number of characters in string s.

  ``` SQL
  SELECT name, capital
  FROM world
  WHERE length(name) = length(capital)
  ```
#### Matching name and capital
  12. Show the *name* and the *capital* where the first letters of each match. Don't include countries where the name and the capital are the same word.

  ``` SQL
  SELECT name, capital
  FROM world
  WHERE  LEFT(name,1) = LEFT(capital, 1) AND name <> capital
  ```
#### All the vowels
  13. Equatorial Guinea and Dominican Republic have all of the vowels (a e i o u) in the name. They don't count because they have more than one word in the name.
  **Find the country that has all the vowels and no spaces in its name.**
  ``` SQL
  SELECT name FROM world
  WHERE name LIKE '%a%'
  AND name LIKE '%e%'
  AND name LIKE '%i%'
  AND name LIKE '%o%'
  AND name LIKE '%u%'
  AND name NOT LIKE '% %'
  ```
