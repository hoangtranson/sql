
6. Query the list of CITY names ending with vowels (a, e, i, o, u) from STATION

```mysql
SELECT DISTINCT CITY FROM STATION WHERE CITY LIKE '%a' OR CITY LIKE '%e' OR CITY LIKE '%i' OR CITY LIKE '%o' OR CITY LIKE '%u';
```

7. Query the list of CITY names from STATION which have vowels (i.e., a, e, i, o, and u) as both their first and last characters. Your result cannot contain duplicates.

```mysql
SELECT DISTINCT CITY FROM station WHERE (CITY LIKE 'a%' OR CITY LIKE 'e%' OR CITY LIKE 'i%' OR CITY LIKE 'o%' OR CITY LIKE 'u%') AND (CITY LIKE '%a' OR CITY LIKE '%e' OR CITY LIKE '%i' OR CITY LIKE '%o' OR CITY LIKE '%u');
```

8. Query the list of CITY names from STATION that do not start with vowels. Your result cannot contain duplicates.

```mysql
SELECT DISTINCT CITY FROM STATION WHERE NOT (CITY LIKE 'a%' OR CITY LIKE 'e%' OR CITY LIKE 'i%' OR CITY LIKE 'o%' OR CITY LIKE 'u%');
```

9. Query the list of CITY names from STATION that do not end with vowels. Your result cannot contain duplicates

```mysql
SELECT DISTINCT CITY FROM STATION WHERE NOT (CITY LIKE '%a' OR CITY LIKE '%e' OR CITY LIKE '%i' OR CITY LIKE '%o' OR CITY LIKE '%u');
```

10. Query the list of CITY names from STATION that either do not start with vowels or do not end with vowels. Your result cannot contain duplicates.

```mysql
SELECT DISTINCT CITY FROM STATION WHERE NOT((CITY LIKE 'a%' OR CITY LIKE 'e%' OR CITY LIKE 'i%' OR CITY LIKE 'o%' OR CITY LIKE 'u%') AND (CITY LIKE '%a' OR CITY LIKE '%e' OR CITY LIKE '%i' OR CITY LIKE '%o' OR CITY LIKE '%u'));
```

11. Query the list of CITY names from STATION that do not start with vowels and do not end with vowels. Your result cannot contain duplicates.


```mysql
SELECT DISTINCT CITY FROM STATION WHERE NOT((CITY LIKE 'a%' OR CITY LIKE 'e%' OR CITY LIKE 'i%' OR CITY LIKE 'o%' OR CITY LIKE 'u%') OR (CITY LIKE '%a' OR CITY LIKE '%e' OR CITY LIKE '%i' OR CITY LIKE '%o' OR CITY LIKE '%u'));
```

12. Query the following two values from the STATION table:

The sum of all values in LAT_N rounded to a scale of  decimal places.
The sum of all values in LONG_W rounded to a scale of  decimal places.

```mysql
select round(sum(lat_n),2), round(sum(long_w),2) from station;
```

13. Query the sum of Northern Latitudes (LAT_N) from STATION having values greater than 38.7880 and less than 137.2345. Truncate your answer to  decimal places.

```mysql
select round(sum(lat_n),4) from station where lat_n between 38.7880 and 137.2345;
```

14. Query the greatest value of the Northern Latitudes (LAT_N) from STATION that is less than 137.2345. Truncate your answer to  decimal places.

```mysql
select round(max(lat_n),4) from station where lat_n < 137.2345;
```

15. Query the Western Longitude (LONG_W) for the largest Northern Latitude (LAT_N) in STATION that is less than 137.2345. Round your answer to  decimal places.

```mysql
select round(long_w, 4) from station where lat_n < 137.2345 order by lat_n desc limit 1;
```

16. Query the smallest Northern Latitude (LAT_N) from STATION that is greater than 38.7780. Round your answer to  decimal places.

```mysql
select round(lat_n, 4) from station where lat_n > 38.7780 order by lat_n asc limit 1;
```

17. Query the Western Longitude (LONG_W)where the smallest Northern Latitude (LAT_N) in STATION is greater than 38.7780. Round your answer to  decimal places.

```mysql
select round(long_w, 4) from station where lat_n > 38.7780 order by lat_n limit 1;
```