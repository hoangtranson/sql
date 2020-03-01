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
combine columns

```sql
SELECT
    e1.first_name || " " || e1.last_name employee_name,
    e1.title employee_title,
    e2.first_name || " " || e2.last_name supervisor_name,
    e2.title supervisor_title
FROM employee e1
LEFT JOIN employee e2 on e1.reports_to = e2.employee_id
ORDER by employee_name asc
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

more complicated inner join

```sql
SELECT 
f.name country,
c.urban_pop,
f.population total_pop,
c.urban_pop/CAST(f.population AS FLOAT) urban_pct
FROM facts f
INNER JOIN (
 SELECT facts_id, SUM(population) urban_pop
 FROM cities 
 GROUP BY 1
) c ON c.facts_id = f.id
where urban_pct > 0.5
order by 4 ASC
```

```sql
SELECT
    ta.album,
    ta.artist_name artist,
    COUNT(*) tracks_purchased
FROM invoice_line il
INNER JOIN (
            SELECT
                t.track_id,
                ar.name artist_name,
                al.title album
            FROM track t
            INNER JOIN album al ON al.album_id = t.album_id
            INNER JOIN artist ar ON ar.artist_id = al.artist_id
           ) ta
           ON ta.track_id = il.track_id
GROUP BY 2
ORDER BY 3 DESC LIMIT 5;
```

inner join same table

```sql
SELECT
    e1.employee_id,
    e2.employee_id supervisor_id
FROM employee e1
INNER JOIN employee e2 on e1.reports_to = e2.employee_id
LIMIT 4;
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

## When keyword

syntax:

```sql
CASE
    WHEN [comparison_1] THEN [value_1]
    WHEN [comparison_2] THEN [value_2]
    ELSE [value_3]
    END
    AS [new_column_name]
```

ex1: 

```sql
SELECT
    media_type_id,
    name,
    CASE
        WHEN name LIKE '%Protected%' THEN 1
        ELSE 0
        END
        AS protected
FROM media_type;
```
ex2: use with inner join

```sql
SELECT
    c.first_name || " " || c.last_name customer_name,
    i.number_of_purchases,
    i.total_spent,
    CASE
        WHEN i.total_spent < 40 THEN 'small spender'
        WHEN i.total_spent > 100 THEN 'big spender'
        ELSE 'regular'
        END
        AS customer_category
FROM customer c
INNER JOIN (
 SELECT 
    customer_id, 
    count(*) number_of_purchases,
    SUM(total) total_spent
 FROM invoice 
 GROUP BY 1
) i ON i.customer_id = c.customer_id
ORDER by customer_name asc
```

## With keyword

Syntax:

```sql
WITH [alias_name] AS ([subquery])

SELECT [main_query]
```
ex1:

```sql
SELECT * FROM
    (
     SELECT
         t.name,
         ar.name artist,
         al.title album_name,
         mt.name media_type,
         g.name genre,
         t.milliseconds length_milliseconds
     FROM track t
     INNER JOIN media_type mt ON mt.media_type_id = t.media_type_id
     INNER JOIN genre g ON g.genre_id = t.genre_id
     INNER JOIN album al ON al.album_id = t.album_id
     INNER JOIN artist ar ON ar.artist_id = al.artist_id
    )
WHERE album_name = "Jagged Little Pill";
```

can change to:

```sql
WITH track_info as
    (
     SELECT
         t.name,
         ar.name artist,
         al.title album_name,
         mt.name media_type,
         g.name genre,
         t.milliseconds length_milliseconds
     FROM track t
     INNER JOIN media_type mt ON mt.media_type_id = t.media_type_id
     INNER JOIN genre g ON g.genre_id = t.genre_id
     INNER JOIN album al ON al.album_id = t.album_id
     INNER JOIN artist ar ON ar.artist_id = al.artist_id
    )

SELECT * from track_info
WHERE album_name = "Jagged Little Pill";
```

ex2: 

```sql
WITH playlist_info as
    (
        SELECT
            p.playlist_id,
            p.name playlist_name,
            t.name track_name,
            (t.milliseconds/1000) length_seconds
        FROM playlist p
        LEFT JOIN playlist_track pt ON pt.playlist_id = p.playlist_id
        LEFT JOIN track t ON t.track_id = pt.track_id
    )

SELECT
    playlist_id,
    playlist_name,
    COUNT(track_name) number_of_tracks,
    SUM(length_seconds) length_seconds
FROM playlist_info
GROUP BY 1
ORDER by 1
```

## create view

```sql
CREATE VIEW chinook.customer_gt_90_dollars AS 
     SELECT c.* 
     FROM chinook.invoice i
     INNER JOIN chinook.customer c
     ON i.customer_id = c.customer_id
     GROUP BY 1
     HAVING SUM(i.total) > 90;
     
SELECT * FROM chinook.customer_gt_90_dollars;
```
