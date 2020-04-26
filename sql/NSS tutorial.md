# NSS Tutorial

![](https://github.com/jasonmchlee/sql-practice/blob/master/sql/table3.png)

### Show the the percentage who STRONGLY AGREE

```sql
SELECT A_STRONGLY_AGREE
  FROM nss
 WHERE question='Q01'
   AND institution='Edinburgh Napier University'
   AND subject='(8) Computer Science';
```

### Show the institution and subject where the score is at least 100 for question 15.

```sql
SELECT institution, subject
FROM nss
WHERE question='Q15' AND score >= 100
```

### Show the subject and total number of students who responded to question 22 for each of the subjects '(8) Computer Science' and '(H) Creative Arts and Design'

```sql
SELECT subject, SUM(response)
  FROM nss
 WHERE question='Q22'
   AND (subject='(H) Creative Arts and Design'
   OR subject='(8) Computer Science')
GROUP BY subject
```

### Show the subject and total number of students who A_STRONGLY_AGREE to question 22 for each of the subjects '(8) Computer Science' and '(H) Creative Arts and Design'.

```sql
SELECT subject, SUM((A_STRONGLY_AGREE / 100) * response)
FROM nss
WHERE
   question='Q22' AND
   (subject='(H) Creative Arts and Design'
   OR subject='(8) Computer Science')
GROUP BY subject
```

### Show the percentage of students who A_STRONGLY_AGREE to question 22 for the subject '(8) Computer Science' show the same figure for the subject '(H) Creative Arts and Design'

```sql
SELECT subject, ROUND(SUM(A_STRONGLY_AGREE * response) / SUM(response))
FROM nss
WHERE
   question='Q22' AND
   (subject='(H) Creative Arts and Design'
   OR subject='(8) Computer Science')
GROUP BY subject
```

### Show the average scores for question 'Q22' for each institution that include 'Manchester' in the name.

```sql
SELECT institution, ROUND(SUM(score*response) / SUM(response))
FROM nss
WHERE 
	question='Q22'
  AND (institution LIKE '%Manchester%')
GROUP BY institution
ORDER BY institution
```

### Show the institution, the total sample size and the number of computing students for institutions in Manchester for 'Q01'.

```sql
SELECT institution, SUM(sample) AS size, 
    SUM(CASE WHEN subject = '(8) Computer Science' THEN sample ELSE 0 END) AS CS_students
FROM nss
WHERE institution LIKE '%Manchester%'
	AND question='Q01'
GROUP BY institution;
```
