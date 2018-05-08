### The JOIN operation(SQLZOO)

[TOC]

![ERdiagram](http://zh.sqlzoo.net/w/images/c/ce/FootballERD.png)

#### JOIN and UEFA EURO 2012
The tables contain all matches and goals from UEFA EURO 2012 Football Championship in Poland and Ukraine.

1. Show the matchid and player name for all goals scored by Germany. To identify German players, check for: ```teamid = 'GER'```

    ```sql
    SELECT matchid, player
    FROM goal
    WHERE teamid = 'GER'
    ```

2. From the previous query you can see that Lars Bender's scored a goal in game 1012. Now we want to know what teams were playing in that match.

    - Notice in the that the column ```matchid``` in the ```goal``` table corresponds to the ```id``` column in the ```game``` table. We can look up information about game 1012 by finding that row in the **game** table.

    - Show id, stadium, team1, team2 for just game 1012

    ```sql
    SELECT id, stadium, team1, team2
    FROM game
    WHERE id = 1012
    ```

3. You can combine the two steps into a single query with a ```JOIN```.

    ```sql
    SELECT *
    FROM game JOIN goal ON (id=matchid)
    ```
    The **FROM** clause says to merge data from the goal table with that from the game table. The ON says how to figure out which rows in **game** go with which rows in **goal** - the **id** from **goal** must match **matchid** from **game**. (If we wanted to be more clear/specific we could say

    ```sql
    ON (game.id=goal.matchid)
    ```
    Modify it to show the player, teamid, stadium and mdate and for every German goal.

    ```sql
    SELECT player, teamid, stadium, mdate
    FROM goal JOIN game ON (id = matchid)
    WHERE teamid = 'GER'
    ```

4. Show the team1, team2 and player for every goal scored by a player called Mario

    ```sql
    SELECT game.team1,game.team2, goal.player
    FROM goal
    JOIN game
    ON goal.matchid = game.id
    WHERE player LIKE 'Mario%'
    ```

5. Show ```player```, ```teamid```, ```coach```, ```gtime``` for all goals scored in the first 10 minutes ```gtime<=10```

    ```sql
    SELECT player, teamid, coach, gtime
    FROM goal
    JOIN eteam
    ON goal.teamid = eteam.id
    WHERE gtime<=10
    ```

6. List the the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach.

    ```sql
    SELECT mdate, eteam.teamname
    FROM game
    JOIN eteam
    ON game.team1 = eteam.id
    WHERE eteam.coach = 'Fernando Santos'
    ```

7. List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'

    ```sql
    SELECT player
    FROM goal
    JOIN game
    ON goal.matchid = game.id
    WHERE game.stadium = 'National Stadium, Warsaw'
    ```

#### More difficult questions

8. Show the name of all players who scored a goal against Germany.

    Select goals scored only by non-German players in matches where GER was the id of either **team1** or **team2**.

    You can use ```teamid!='GER'``` to prevent listing German players.

    You can use ```DISTINCT``` to stop players being listed twice.

    ```sql
    SELECT DISTINCT(player)
    FROM goal JOIN game
    ON goal.matchid = game.id
    WHERE goal.teamid != 'GER'
    AND (game.team1 = 'GER'
    OR game.team2 = 'GER')
    ```

9. Show teamname and the total number of goals scored.

    ```sql
    SELECT eteam.teamname, COUNT(*) AS total_goals
    FROM eteam JOIN goal
    ON eteam.id = goal.teamid
    GROUP BY eteam.teamname
    ```

10. Show the stadium and the number of goals scored in each stadium.

    ```sql
    SELECT stadium, COUNT(goal.matchid) AS goals
    FROM game JOIN goal
    ON game.id = goal.matchid
    GROUP BY stadium
    ```

11. For every match involving 'POL', show the matchid, date and the number of goals scored.

    ```sql
    SELECT game.id, game.mdate, COUNT(goal.matchid) AS number_goals
    FROM game JOIN goal ON goal.matchid = game.id
    WHERE (team1 = 'POL' OR team2 = 'POL')
    GROUP BY game.id, game.mdate
    ORDER BY game.id
    ```

12. For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'

    ```sql
    SELECT game.id, game.mdate, COUNT(goal.matchid)
    FROM game
    JOIN goal
    ON game.id = goal.matchid
    WHERE goal.teamid = 'GER'
    GROUP BY game.id, game.mdate
    ```

13. List every match with the goals scored by each team as shown. This will use "```CASE WHEN```"

    ```sql
    SELECT mdate,
           team1,
           SUM(CASE WHEN teamid = team1 THEN 1 ELSE 0 END) AS score1,
           team2,
           SUM(CASE WHEN teamid = team2 THEN 1 ELSE 0 END) AS score2
    FROM game LEFT JOIN goal ON (id = matchid)
    GROUP BY mdate,team1,team2
    ORDER BY mdate, matchid, team1, team2
    ```
