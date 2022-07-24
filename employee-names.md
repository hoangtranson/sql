# Table

- employee_id
- name
- months
- salary

Write a query that prints a list of employee names (i.e.: the name attribute) from the Employee table in alphabetical order.

```mysql
select name from employee order by name;
```

Write a query that prints a list of employee names (i.e.: the name attribute) for employees in Employee having a salary greater than 2000$ per month who have been employees for less than 10 months. Sort your result by ascending employee_id.

```mysql
select name from employee where salary > 2000 and months < 10 order by employee_id;
```

Samantha was tasked with calculating the average monthly salaries for all employees in the EMPLOYEES table, but did not realize her keyboard's 0 key was broken until after completing the calculation. She wants your help finding the difference between her miscalculation (using salaries with any zeros removed), and the actual average salary.

Write a query calculating the amount of error (i.e.: actual - miscalculated average monthly salaries), and round it up to the next integer.

```mysql
select ceil(avg(salary) - avg(replace(salary, '0', ''))) from employees;
```

We define an employee's total earnings to be their monthly salary x months worked, and the maximum total earnings to be the maximum total earnings for any employee in the Employee table. Write a query to find the maximum total earnings for all employees as well as the total number of employees who have maximum total earnings. Then print these values as 2 space-separated integers.


```mysql
select max(salary*months), count(*) from employee where (salary*months) = (select max(salary*months) from employee);
```