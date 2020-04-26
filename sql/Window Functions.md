# Window Functions

General Elections were held in the UK in 2015 and 2017. Every citizen votes in a **constituency**. The candidate who gains the most votes becomes MP for that constituency.

All these results are recorded in a table **ge**


### Show the lastName, party and votes for the constituency 'S14000024' in 2017.

```sql
SELECT lastName, party, votes
  FROM ge
 WHERE constituency = 'S14000024' AND yr = 2017
ORDER BY votes DESC
```

### Show the party and RANK for constituency S14000024 in 2017

```sql
SELECT party, votes,
       RANK() OVER (ORDER BY votes DESC) as posn
FROM ge
WHERE constituency = 'S14000024' AND yr = 2017
ORDER BY party
```

### Show the ranking of each party in S14000021 in each year. Include yr, party, votes and ranking

```sql
SELECT yr, party, votes,
 RANK() OVER(PARTITION BY yr ORDER BY votes DESC)
FROM ge
WHERE constituency = 'S14000021'
ORDER BY party, yr
```

### Show the ranking of each party in Edinburgh in 2017

- Edinburgh constituencies are numbered S14000021 to S14000026.

```sql
WITH edinburgh AS
(SELECT *
FROM ge
WHERE (constituency between 'S14000021' AND 'S14000026') AND yr = 2017)

SELECT
  constituency,
  party,
  votes,
  RANK() OVER(PARTITION BY constituency ORDER BY votes DESC) as rank
FROM edinburgh
ORDER BY rank, constituency
```

### Show the parties that won for each Edinburgh constituency in 2017.

```sql
SELECT 
   constituency,
   party

FROM
     (SELECT constituency,party, votes,
      RANK() OVER (PARTITION BY constituency ORDER BY votes DESC) as position
      FROM ge
      WHERE constituency BETWEEN 'S14000021' AND 'S14000026'
      AND yr  = 2017) ranking

WHERE position=1
```

### Show how many seats for each party in Scotland in 2017.

```sql
WITH scotland AS
(SELECT
 party,
 constituency,
 RANK() OVER(PARTITION BY constituency ORDER BY votes DESC) AS position
FROM ge
WHERE constituency LIKE 'S%' AND yr = 2017)

SELECT
 party,
 COUNT(*)
FROM scotland
WHERE position = 1
GROUP BY party
```
