# sql
sample query on sql

1. left join

```sql
SELECT f.name country, c.name city
FROM facts f
LEFT JOIN cities c ON c.facts_id = f.id
LIMIT 5;
```

2. right join

```sql
SELECT f.name country, c.name city
FROM cities c
RIGHT JOIN facts f ON f.id = c.facts_id
LIMIT 5;
```

3. full outer join

```sql
SELECT f.name country, c.name city
FROM cities c
FULL OUTER JOIN facts f ON f.id = c.facts_id
LIMIT 5;
```

4. inner join

ex1:

```sql
SELECT c.name capital_city, f.name country, c.population population FROM FACTS f
INNER JOIN cities c
on c.facts_id = f.id
where c.capital is 1
ORDER by 3 desc
limit 10
```

ex2:


```sql
SELECT c.name capital_city, f.name country, c.population population FROM FACTS f
INNER JOIN cities c
on c.facts_id = f.id
where c.capital is 1 and c.population > 10000000
ORDER by 3 desc
```

ex3: sub query

```sql
SELECT c.name capital_city, f.name country, c.population population FROM FACTS f
INNER JOIN (
 SELECT * 
 FROM cities 
 WHERE capital is 1 and population > 10000000
) c
on c.facts_id = f.id
ORDER by 3 desc
```

