# Sample Query on SQL

This is list of sample query for SQL that help to memorize the SQL

## Generate Table info

```sql
PRAGMA TABLE_INFO(recent_grads);
```

## casting value

```sql
SELECT CAST(Women as Float) / CAST(Total as Float) 
FROM recent_grads;
```

## left join

```sql
SELECT f.name country, c.name city
FROM facts f
LEFT JOIN cities c ON c.facts_id = f.id
LIMIT 5;
```

## right join

```sql
SELECT f.name country, c.name city
FROM cities c
RIGHT JOIN facts f ON f.id = c.facts_id
LIMIT 5;
```

## full outer join

```sql
SELECT f.name country, c.name city
FROM cities c
FULL OUTER JOIN facts f ON f.id = c.facts_id
LIMIT 5;
```

## inner join

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
## Sub queries

ex1:

```sql
SELECT Major, ShareWomen 
FROM recent_grads
WHERE ShareWomen > 
( 
  SELECT AVG(ShareWomen) 
  FROM recent_grads 
)                                                           
```

ex2:

```sql
SELECT *
FROM customers c
WHERE 100 <
(
  SELECT COUNT ( * )
  FROM orders
  WHERE customer_id = c.customer_id
);
```

## In keywork

Return row that matchs list of value

ex1:

```sql
SELECT Major, Major_category 
FROM recent_grads
WHERE Major_category 
IN ('Business', 'Engineering')
```

ex2:

```sql
SELECT *
FROM table_A
WHERE column_X IN
(
  SELECT column_X
  FROM table_B
);
```

## aggregation

ex1: 

```sql
SELECT SUM(Employed)
FROM recent_grads
GROUP BY Major_category;
```

ex2:

```sql
SELECT Major_category, AVG(Employed) / AVG(Total) AS share_employed 
FROM recent_grads 
GROUP BY Major_category 
HAVING share_employed > 0.8;
```

ex3:

```sql
SELECT Major_category, ROUND(ShareWomen, 2) AS rounded_share_women 
FROM recent_grads;
```
