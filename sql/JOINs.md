# JOINs
![](https://github.com/jasonmchlee/sql-practice/blob/master/sql/db1.png)
![](https://github.com/jasonmchlee/sql-practice/blob/master/sql/table2.png)

### Identify German players

```sql
SELECT matchid, player 
FROM goal 
  WHERE teamid = 'GER'
```

### Show id, stadium, team1, team2 for just game 1012

```sql
SELECT id,stadium,team1,team2
  FROM game
WHERE id = 1012
```

### Show the player, teamid, stadium and mdate for every German goal.

```sql
SELECT go.player, go.teamid, g.stadium, g.mdate
FROM game as g
JOIN goal as go
	ON g.id = go.matchid
WHERE go.teamid = 'GER'
```

### Show the team1, team2 and player for every goal scored by a player called Mario

```sql
SELECT g.team1, g.team2, go.player
FROM game as g
JOIN goal as go
   ON g.id = go.matchid
WHERE go.player LIKE 'Mario%'
```

### Show player, teamid, coach, gtime for all goals scored in the first 10 minutes gtime<=10

```sql
SELECT g.player, g.teamid, e.coach, g.gtime
FROM goal AS g
JOIN eteam AS e
  ON g.teamid = e.id
WHERE g.gtime <= 10
```

### List the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach.

```sql
SELECT g.mdate, e.teamname
FROM game AS g
JOIN eteam AS e
  ON e.id = g.team1
WHERE e.coach = 'Fernando Santos'
```

### List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'

```sql
SELECT go.player
FROM goal AS go
JOIN game AS g
  ON go.matchid = g.id
WHERE g.stadium  = 'National Stadium, Warsaw'
```

### Show the name of all players who scored a goal against Germany

```sql
SELECT DISTINCT(player)
FROM game 
JOIN goal 
ON matchid = id 
WHERE (team1='GER' or team2='GER') AND teamid != 'GER'
```

### Show teamname and the total number of goals scored.

```sql
SELECT teamname, COUNT(matchid)
FROM eteam AS e
JOIN goal AS g
ON g.teamid = e.id
GROUP BY teamname
```

### Show the stadium and the number of goals scored in each stadium.

```sql
SELECT stadium, COUNT(matchid)
FROM game AS g
JOIN goal AS go
ON go.matchid = g.id
GROUP BY stadium
```

### For every match involving 'POL', show the matchid, date and the number of goals scored

```sql
SELECT matchid, mdate, COUNT(matchid)
FROM game as g
JOIN goal as go
ON g.id = go.matchid
WHERE team1 = 'POL' OR team2 = 'POL'
GROUP BY matchid, mdate
```

### For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'

```sql
SELECT matchid, mdate, COUNT(matchid) as goal_scored
FROM game as g
JOIN goal as go
  ON g.id = go.matchid
WHERE teamid = 'GER'
GROUP BY matchid, mdate
```

### List every match with the goals scored by each team as shown.

```sql
SELECT mdate,
  team1,
  SUM(CASE WHEN teamid=team1 THEN 1 ELSE 0 END) AS score1,
  team2,
  SUM(CASE WHEN teamid=team2 THEN 1 ELSE 0 END) AS score2
FROM game 
LEFT JOIN goal 
ON matchid = id
GROUP BY mdate, team1, team2
ORDER BY mdate, matchid, team1, team2
```
