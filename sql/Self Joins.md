# Self Joins

![](https://github.com/jasonmchlee/sql-practice/blob/master/sql/table4.png)


### How many stops are in the database.

```sql
SELECT COUNT(id)
FROM stops
```

### Find the id value for the stop 'Craiglockhart'

```sql
SELECT id
FROM stops
WHERE name = 'Craiglockhart'
```

### Give the id and the name for the stops on the '4' 'LRT' service.

```sql
SELECT id, name 
FROM stops 
JOIN route 
 ON id = stop
WHERE num = '4' AND company = 'LRT'
```

### Find routes that visit either London Road (149) or Craiglockhart (53) and have more than 2 counts

```sql
SELECT company, num, COUNT(*)
FROM route 
WHERE stop=149 OR stop=53
GROUP BY company, num
HAVING COUNT(*) > 1
```

### Use a Self-join to find the start stop as Craiglockhart and end stop as London Road

```sql
SELECT a.company, a.num, a.stop AS start, b.stop AS end
FROM route AS a
JOIN route AS b
  ON a.num = b.num
WHERE a.stop=53 and b.stop = 149
```

### Give a list of all the services which connect stops 115 and 137 ('Haymarket' and 'Leith')

```sql
SELECT DISTINCT a.company, a.num
FROM route AS a 
JOIN route AS b ON (a.num = b.num)
WHERE a.stop= 115 AND b.stop = 137
```

### Give a list of the services which connect the stops 'Craiglockhart' and 'Tollcross'

```sql
SELECT DISTINCT a.company, a.num
FROM route AS a
JOIN route AS b
  ON a.num = b.num
JOIN stops AS sa
  ON sa.id = a.stop
JOIN stops AS sb
  ON sb.id = b.stop
WHERE sa.name ='Craiglockhart' AND sb.name ='Tollcross'
```
