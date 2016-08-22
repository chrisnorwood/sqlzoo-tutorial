# SQL Zoo Tutorial Solutions
These are the solutions to the [SQLZoo SQL Tutorial](http://sqlzoo.net/wiki/SQL_Tutorial)

## Table of Contents
0. [SELECT Basics](#select-basics)
1. [SELECT from world](#select-from-world)
2. [SELECT from nobel](#select-from-nobel)
3. [SELECT within SELECT](#select-within-select)
4. [SUM and COUNT](#sum-and-count)
5. [The JOIN Operation](#the-join-operation)
6. [More JOIN](#more-join)
7. [Using NULL](#using-null)
8. [Self join](#self-join)

## SELECT Basics
### 1.
```SQL
SELECT population FROM world
  WHERE name = 'Germany';
```

### 2.
```SQL
SELECT name, population FROM world
  WHERE name IN ('Ireland', 'Denmark', 'Iceland');
```

### 3.
```SQL
SELECT name, area FROM world
  WHERE area BETWEEN 200000 AND 250000;
```

## SELECT from world
### 1.
```SQL
SELECT name, continent, population FROM world;
```

### 2.
```SQL
SELECT name FROM world
WHERE population>200000000;
```

### 3.
```SQL
SELECT name, gdp/population
FROM world
WHERE population > 200000000;
```

### 4.
```SQL
SELECT name, population/1000000 AS population_in_millions
FROM world
WHERE continent = 'South America';
```

### 5.
```SQL
SELECT name, population
FROM world
WHERE name IN ('France', 'Germany', 'Italy');
```

### 6.
```SQL
SELECT name
FROM world
WHERE name LIKE '%United%';
```

### 7.
```SQL
SELECT name, population, area
FROM world
WHERE population>250000000 OR area>3000000;
```

### 8.
```SQL
SELECT name, population, area
FROM world
WHERE population>250000000 XOR area>3000000;
```

### 9.
```SQL
SELECT name, 
       ROUND(population/1000000, 2) AS population_in_millions,
       ROUND(gdp/1000000000,2) AS gdp_in_billions
FROM world
WHERE continent = 'South America';
```

### 10.
```SQL
SELECT name, ROUND(gdp/population, -3) AS gdp_per_capita
FROM world
WHERE gdp >1000000000000;
```

### 11.
```SQL
SELECT name,
    CASE WHEN continent='Oceania' THEN 'Australasia'
         ELSE continent END
FROM world
WHERE name LIKE 'N%';
```

### 12.
```SQL
SELECT name,
    CASE WHEN continent='Europe' OR continent='Asia' THEN 'Eurasia'
         WHEN continent IN ('North America', 'South America', 'Caribbean') THEN 'America'
         ELSE continent END
FROM world
WHERE name LIKE 'A%' OR name LIKE 'B%';
```

### 13.
```SQL
meh
```

## SELECT from Nobel
### 1.
```SQL
SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950;
```

### 2.
```SQL
SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'Literature';
```

### 3.
```SQL
SELECT yr, subject
FROM nobel
WHERE winner = 'Albert Einstein';
```

### 4.
```SQL
SELECT winner
FROM nobel
WHERE subject = 'Peace' AND yr >= 2000;
```

### 5.
```SQL
SELECT *
FROM nobel
WHERE subject = 'Literature' AND yr BETWEEN 1980 and 1989;
```

### 6.
```SQL
SELECT * 
FROM nobel
WHERE winner IN ('Theodore Roosevelt', 'Woodrow Wilson', 'Jimmy Carter');
```

### 7.
```SQL
SELECT winner
FROM nobel
WHERE winner LIKE 'John%';
```

### 8.
```SQL
SELECT *
FROM nobel
WHERE (subject = 'Physics' AND yr = 1980) 
   OR (subject = 'Chemistry' AND yr = 1984);
```

### 9.
```SQL
SELECT *
FROM nobel
WHERE yr = 1980 AND subject NOT IN ('Chemistry', 'Medicine');
```

### 10.
```SQL
SELECT *
FROM nobel
WHERE (yr < 1910  AND subject = 'Medicine')
   OR (yr >= 2004 AND subject = 'Literature');
```

### 11.
```SQL
SELECT *
FROM nobel
WHERE winner LIKE '%Grün%';
```

### 12.
```SQL
SELECT *
FROM nobel
WHERE winner = 'Eugene O\'Neill';
```

### 13.
```SQL
SELECT winner, yr, subject
FROM nobel
WHERE winner LIKE 'Sir %'
ORDER BY yr DESC, winner;
```

### 14.
```SQL
SELECT winner, subject
FROM nobel
WHERE yr=1984
ORDER BY subject IN ('Physics','Chemistry'), subject, winner;
```

## SELECT within SELECT
### 1.
```SQL
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia');
```

### 2.
```SQL
SELECT name FROM world
    WHERE continent='Europe' 
      AND gdp/population >
          (SELECT gdp/population FROM world
           WHERE name='United Kingdom');
```

### 3.
```SQL
SELECT name, continent
FROM world
WHERE continent IN (SELECT continent FROM world
                    WHERE name IN ('Argentina', 'Australia'))
ORDER BY name;
```

### 4.
```SQL
SELECT name, population
FROM world
WHERE population > (SELECT population FROM world WHERE name = 'Canada')
  AND population < (SELECT population FROM world WHERE name = 'Poland');
```

### 5.
```SQL
SELECT name, CONCAT(ROUND(population/(SELECT population FROM world 
                                      WHERE name='Germany')*100.0),'%')
FROM world
WHERE continent = 'Europe';
```

### 6.
```SQL
SELECT name
FROM world
WHERE gdp >
      (SELECT MAX(gdp) FROM world WHERE continent = 'Europe');
```

### 7.
```SQL
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
     WHERE y.continent=x.continent
     AND area>0);
```

### 8.
```SQL
SELECT continent, name FROM world x
WHERE name <= ALL
      (SELECT name FROM world y WHERE y.continent = x.continent)
```

### 9.
```SQL
SELECT name, continent, population
FROM world x
WHERE 25000000 >= ALL 
      (SELECT population FROM world y WHERE y.continent=x.continent);
```

### 10.
```SQL
SELECT name, continent
FROM world x
WHERE population > ALL 
      (SELECT population*3 FROM world y 
       WHERE y.continent=x.continent
       AND y.name <> x.name);
```

## SUM and COUNT
### 1.
```SQL
SELECT SUM(population)
FROM world;
```

### 2.
```SQL
SELECT DISTINCT continent
FROM world;
```

### 3.
```SQL
SELECT SUM(gdp)
FROM world
WHERE continent = 'Africa';
```

### 4.
```SQL
SELECT COUNT(name)
FROM world
WHERE area >=1000000;
```

### 5. 
```SQL
SELECT SUM(population)
FROM world
WHERE name IN ('France', 'Germany', 'Spain');
```

### 6.
```SQL
SELECT continent, COUNT(name)
FROM world
GROUP BY continent;
```

### 7.
```SQL
SELECT continent, COUNT(population)
FROM world
WHERE population>=10000000
GROUP BY continent;
```

### 8.
```SQL
SELECT continent
FROM world
GROUP BY continent
HAVING SUM(population) > 100000000;
```

## The JOIN Operation
### 1.
```SQL
SELECT matchid, player FROM goal 
WHERE teamid = 'GER';
```

### 2.
```SQL
SELECT id,stadium,team1,team2
FROM game
WHERE id = 1012;
```

### 3.
```SQL
SELECT goal.player, goal.teamid, game.stadium, game.mdate
FROM game JOIN goal ON (id=matchid)
WHERE goal.teamid = 'GER';
```

### 4.
```SQL
SELECT game.team1, game.team2, goal.player
FROM game JOIN goal ON (game.id = goal.matchid)
WHERE player LIKE 'Mario%';
```

### 5.
```SQL
SELECT goal.player, goal.teamid, eteam.coach, goal.gtime
FROM goal JOIN eteam on (goal.teamid=eteam.id)
WHERE goal.gtime<=10;
```

### 6.
```SQL
SELECT game.mdate, eteam.teamname
FROM game JOIN eteam ON (game.team1=eteam.id)
WHERE eteam.coach = 'Fernando Santos';
```

### 7.
```SQL
SELECT goal.player
FROM goal JOIN game ON (goal.matchid=game.id)
WHERE game.stadium =  'National Stadium, Warsaw';
```

### 8.
```SQL
SELECT DISTINCT goal.player
FROM game JOIN goal ON goal.matchid = game.id 
WHERE (game.team1='GER' OR game.team2='GER')
       AND goal.teamid <> 'GER';
```

### 9.
```SQL
SELECT eteam.teamname, COUNT(goal.teamid)
FROM eteam JOIN goal ON eteam.id=goal.teamid
GROUP BY teamname;
```

### 10.
```SQL
SELECT game.stadium, COUNT(goal.teamid)
FROM game JOIN goal ON game.id=goal.matchid
GROUP BY game.stadium;
```

### 11.
```SQL
SELECT goal.matchid, game.mdate, COUNT(goal.teamid)
FROM game JOIN goal ON goal.matchid = game.id 
WHERE (team1 = 'POL' OR team2 = 'POL')
GROUP BY mdate, matchid;
```

### 12.
```SQL
SELECT goal.matchid, game.mdate, COUNT(goal.teamid)
FROM goal JOIN game ON goal.matchid = game.id
WHERE (goal.teamid = 'GER')
GROUP BY goal.matchid, game.mdate;
```

### 13.
```SQL
SELECT mdate,
       team1,
       SUM(CASE WHEN goal.teamid=game.team1 THEN 1 ELSE 0 END) score1,
       team2,
       SUM(CASE WHEN goal.teamid=game.team2 THEN 1 ELSE 0 END) score2
FROM game 
LEFT JOIN goal ON goal.matchid = game.id
GROUP BY mdate, team1, team2
ORDER BY mdate, team1, team2;
```

## More JOIN
### 1.
```SQL
SELECT id, title
FROM movie
WHERE yr=1962;
```

### 2.
```SQL
SELECT yr
FROM movie
WHERE title='Citizen Kane';
```

### 3.
```SQL
SELECT id, title, yr
FROM movie
WHERE title LIKE '%Star Trek%'
ORDER BY yr;
```

### 4.
```SQL
SELECT title
FROM movie
WHERE id IN (11768, 11955, 21191);
```

### 5.
```SQL
SELECT id
FROM actor
WHERE name = 'Glenn Close';
```

### 6.
```SQL
SELECT id
FROM movie
WHERE title = 'Casablanca';
```

### 7.
```SQL
SELECT actor.name
FROM casting JOIN actor ON casting.actorid = actor.id
WHERE casting.movieid=11768;
```

### 8.
```SQL
SELECT actor.name
FROM casting
JOIN actor ON actor.id = casting.actorid
JOIN movie ON movie.id = casting.movieid
WHERE movie.title = 'Alien';
```

### 9.
```SQL
SELECT movie.title
FROM casting
JOIN actor ON actor.id = casting.actorid
JOIN movie ON movie.id = casting.movieid
WHERE actor.name = 'Harrison Ford';
```

### 10.
```SQL
SELECT movie.title
FROM casting
JOIN actor ON actor.id = casting.actorid
JOIN movie ON movie.id = casting.movieid
WHERE actor.name = 'Harrison Ford'
AND casting.ord <> 1;
```

### 11.
```SQL
SELECT movie.title, actor.name
FROM casting
JOIN actor ON actor.id = casting.actorid
JOIN movie ON movie.id = casting.movieid
WHERE movie.yr = 1962
AND casting.ord = 1;
```

### 12.
```SQL
SELECT yr,COUNT(title) FROM
  movie JOIN casting ON movie.id=movieid
         JOIN actor   ON actorid=actor.id
WHERE name='John Travolta'
GROUP BY yr
HAVING COUNT(title)=(SELECT MAX(c) FROM
(SELECT yr,COUNT(title) AS c FROM
   movie JOIN casting ON movie.id=movieid
         JOIN actor   ON actorid=actor.id
 WHERE name='John Travolta'
 GROUP BY yr) AS t
)
```

### 13.
```SQL
SELECT movie.title, actor.name 
FROM casting
JOIN actor ON actor.id = casting.actorid
JOIN movie ON movie.id = casting.movieid
WHERE ord=1
AND movie.id IN (
  SELECT movie.id FROM casting
  JOIN actor ON actor.id = casting.actorid
  JOIN movie ON movie.id = casting.movieid
  WHERE actor.name='Julie Andrews');
```

### 14.
```SQL
SELECT DISTINCT actor.name
FROM casting
JOIN actor ON actor.id = casting.actorid
JOIN movie ON movie.id = casting.movieid
WHERE casting.actorid IN (
    SELECT actorid FROM casting
    WHERE ord = 1
    GROUP BY actorid
    HAVING count(actorid) >= 30
    )
ORDER BY actor.name;
```

### 15.
```SQL
SELECT movie.title, COUNT(casting.actorid)
FROM casting
JOIN movie ON movie.id = casting.movieid
WHERE movie.yr = 1978
GROUP BY casting.movieid, movie.title
ORDER BY COUNT(casting.actorid) DESC;
```

### 16.
```SQL
SELECT actor.name
FROM actor
JOIN casting ON actor.id = casting.actorid
WHERE actor.name <> 'Art Garfunkel'
AND casting.movieid IN
    (SELECT movie.id FROM movie
    JOIN casting ON movieid = movie.id
    JOIN actor ON actorid = actor.id
    WHERE actor.name='Art Garfunkel');
```

## Using NULL
### 1.
```SQL
SELECT name
FROM teacher
WHERE dept IS NULL;
```

### 2.
```SQL
SELECT teacher.name, dept.name
FROM teacher INNER JOIN dept
    ON (teacher.dept=dept.id);
```

### 3.
```SQL
SELECT teacher.name, dept.name
FROM teacher LEFT JOIN dept
    ON (teacher.dept=dept.id);
```

### 4.
```SQL
SELECT teacher.name, dept.name
FROM teacher RIGHT JOIN dept
    ON (teacher.dept=dept.id);
```

### 5.
```SQL
SELECT name, COALESCE(mobile, '07986 444 2266')
FROM teacher;
```

### 6.
```SQL
SELECT teacher.name, COALESCE(dept.name, 'None')
FROM teacher
LEFT JOIN dept ON dept.id = teacher.dept;
```

### 7.
```SQL
SELECT COUNT(name), COUNT(mobile)
FROM teacher;
```

### 8.
```SQL
SELECT dept.name, COUNT(teacher.name)
FROM teacher RIGHT JOIN dept ON teacher.dept = dept.id
GROUP BY dept.name;
```

### 9.
```SQL
SELECT name, 
       CASE WHEN dept IN (1,2) THEN 'Sci' 
       ELSE 'Art' END
FROM teacher;
```

### 10.
```SQL
SELECT name, 
       CASE WHEN dept IN (1,2) THEN 'Sci' 
       WHEN dept = 3 THEN 'Art'
       ELSE 'None' END
FROM teacher;
```

## Self join
### 1.
```SQL
SELECT COUNT(*) FROM stops;
```

### 2.
```SQL
SELECT id
FROM stops
WHERE name = 'Craiglockhart';
```

### 3.
```SQL
SELECT stops.id, stops.name
FROM stops
JOIN route ON route.stop = stops.id
WHERE route.num = 4;
```

### 4.
```SQL
SELECT company, num, COUNT(*)
FROM route WHERE stop=149 OR stop=53
GROUP BY company, num
HAVING count(*) = 2;
```

### 5.
```SQL
SELECT a.company, a.num, a.stop, b.stop
FROM route a JOIN route b ON (a.company=b.company AND a.num=b.num)
WHERE a.stop = (SELECT id FROM stops WHERE name = 'Craiglockhart') 
  AND b.stop = (SELECT id FROM stops WHERE name = 'London Road');
```

### 6.
```SQL
SELECT a.company, a.num, stopa.name, stopb.name
FROM route a 
    JOIN route b ON (a.company=b.company AND a.num=b.num)
    JOIN stops stopa ON (a.stop=stopa.id)
    JOIN stops stopb ON (b.stop=stopb.id)
WHERE stopa.name='Craiglockhart' 
  AND stopb.name='London Road';
```

### 7.
```SQL
SELECT DISTINCT a.company, a.num
FROM route a 
JOIN route b ON (a.company = b.company AND a.num = b.num)
WHERE a.stop = 115
  AND b.stop = 137;
```

### 8.
```SQL
SELECT a.company, a.num
FROM route a
    JOIN route b ON (a.company = b.company AND a.num = b.num)
    JOIN stops stopa ON a.stop = stopa.id
    JOIN stops stopb ON b.stop = stopb.id
WHERE stopa.name = 'Craiglockhart'
  AND stopb.name = 'Tollcross';
```

### 9.
```SQL
SELECT DISTINCT name, a.company, a.num
FROM route a
JOIN route b ON (a.company = b.company AND a.num = b.num)
JOIN stops ON a.stop = stops.id
WHERE b.stop = 53;
```

### 10.
(Solution from [fcarrara](https://github.com/fcarrara/sql_zoo/blob/master/9_self_join.sql))
```SQL
SELECT DISTINCT an, ac, stops.name, dn, dc
FROM (SELECT a.num as an, a.company as ac, b.stop as bstop
      FROM route a JOIN route b 
      JOIN stops s ON a.num=b.num AND s.id=a.stop
      WHERE s.name='Craiglockhart') T1
JOIN (SELECT d.num as dn, d.company as dc, c.stop as cstop
      FROM route c JOIN route d 
      JOIN stops s ON c.num=d.num AND c.company=d.company AND s.id=d.stop
      WHERE s.name='Sighthill') T2
JOIN stops ON bstop=cstop AND bstop=stops.id
```