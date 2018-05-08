### More JOIN operations(SQLZOO)

[TOC]

![ERdiagram](http://zh.sqlzoo.net/w/images/1/10/Movie-er.png)


1. List the films where the yr is 1962 [Show id, title]

    ```sql
    SELECT id, title
    FROM movie
    WHERE yr = 1962
    ```

2. Give year of 'Citizen Kane'.

    ```sql
    SELECT yr
    FROM movie
    WHERE title = 'Citizen Kane'
    ```

3. List all of the Star Trek movies, include the id, title and yr (all of these movies include the words Star Trek in the title). Order results by year.

    ```sql
    SELECT id, title, yr
    FROM movie
    WHERE title LIKE '%Star Trek%'
    ORDER BY yr
    ```

4. What id number does the actor 'Glenn Close' have?

    ```sql
    SELECT id
    FROM actor
    WHERE name = 'Glenn Close'
    ```

5. What is the id of the film 'Casablanca'

    ```sql
    SELECT id
    FROM movie
    WHERE title = 'Casablanca'
    ```

6. Obtain the cast list for 'Casablanca'.
   what is a cast list?
   Use movieid=11768, (or whatever value you got from the previous question)

    ```sql
    SELECT actor.name
    FROM actor JOIN casting
    ON actorid = actor.id
    WHERE movieid = 11768
    ```

7. Obtain the cast list for the film 'Alien'

    ```sql
    SELECT actor.name
    FROM (actor JOIN casting
    ON actorid = actor.id)
    JOIN movie
    ON movieid = movie.id
    WHERE movie.title = 'Alien'
    ```

8. List the films in which 'Harrison Ford' has appeared

    ```sql
    SELECT title
    FROM (movie JOIN casting
    ON movie.id = casting.movieid)
    JOIN actor
    ON actor.id = casting.actorid
    WHERE actor.name = 'Harrison Ford'
    ```

9. List the films where 'Harrison Ford' has appeared - but not in the starring role. Note: the ord field of casting gives the position of the actor. If ord=1 then this actor is in the starring role

    ```sql
    SELECT title
    FROM (movie JOIN casting
    ON movie.id = casting.movieid)
    JOIN actor
    ON actor.id = casting.actorid
    WHERE actor.name = 'Harrison Ford'
    AND casting.ord != 1
    ```

10. List the films together with the leading star for all 1962 films.

    ```sql
    SELECT yr,COUNT(title)
    FROM (movie
          JOIN casting
          ON movie.id=movieid)
    JOIN actor
    ON actorid=actor.id
    WHERE name='John Travolta'
    GROUP BY yr
    HAVING COUNT(title) > 2
    ```

11. Which were the busiest years for 'John Travolta', show the year and the number of movies he made each year for any year in which he made more than 2 movies.

    ```sql
    SELECT yr,COUNT(title)
    FROM (movie
          JOIN casting
          ON movie.id=movieid)
    JOIN actor
    ON actorid=actor.id
    WHERE name='John Travolta'
    GROUP BY yr
    HAVING COUNT(title) > 2
    ```

12. List the film title and the leading actor for all of the films 'Julie Andrews' played in.

     - Did you get "Little Miss Marker twice"?
Julie Andrews starred in the 1980 remake of Little Miss Marker and not the original(1934).

     - Title is not a unique field, create a table of IDs in your subquery

    ```sql
    SELECT title, actor.name
    FROM (movie x
          JOIN casting
          ON x.id=movieid)
    JOIN actor
    ON actorid=actor.id
    WHERE x.id IN (
    SELECT y.id
    FROM (movie y
          JOIN casting
          ON y.id=movieid)
    JOIN actor
    ON actorid=actor.id
    WHERE actor.name = 'Julie Andrews')
    AND casting.ord =1
    ```

13. Obtain a list, in alphabetical order, of actors who've had at least 30 starring roles.

    ```sql
    SELECT actor.name
    FROM actor
    JOIN casting
    ON casting.actorid = actor.id
    WHERE casting.ord = 1
    GROUP BY actor.name
    HAVING COUNT(casting.movieid) >= 30
    ```

14. List the films released in the year 1978 ordered by the number of actors in the cast, then by title.

    ```sql
    SELECT title, COUNT(actorid)
    FROM movie
    JOIN casting
    ON movie.id = casting.movieid
    WHERE yr = 1978
    GROUP BY title
    ORDER BY COUNT(actorid) DESC, title
    ```

15. List all the people who have worked with 'Art Garfunkel'.

    ```sql
    SELECT name
    FROM (movie
          JOIN casting
          ON movie.id=movieid)
    JOIN actor
    ON actorid=actor.id
    WHERE movie.id IN(
        SELECT movie.id
        FROM (movie
                    JOIN casting
                    ON movie.id=movieid)
        JOIN actor
        ON actorid=actor.id
        WHERE actor.name = 'Art Garfunkel'
    )
    AND actor.name != 'Art Garfunkel'
    ```
