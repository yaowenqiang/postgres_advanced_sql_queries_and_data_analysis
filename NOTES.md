# What is PostgreSQL?

+ ANSI-standard SQL
+ General purpose
+ Object-relational database management system(RDBMS)
+ Free and Open source RDBMS
+ Platform independent
+ Very stabable and requires minimum maintance
+ Sarge storage
+ Fast performance
+ PostgreSQL is extensiable : You can define your own data types, index type, plugins etc
+ Active community support

# PostgreSQL System Architecture

+ PostgreSQL use a client-server model
+ PostgreSQL session consists of a program known as server and client process

## Server process(Postgres)

+ Manages the database files
+ Accepts connections to the database from client applications
+ Performs database actions on behalf of the clients

## User's Client (frontend)

+ applications that wants to perform database operations

## import data from mac

```bash
createuser -s postgres
pg_restore --host localhost --port 5432 --username "postgres" --dbname "dvdrental" --no-password  --verbose "dvdrental.tar"
```

# Service Service

> brew install --cask pgadmin4

# Schema

+ Logical contains of tables and other database objects
+ You can have multiple Schema in a database

# Table space

+ Storage location where data is stored
+ PostgreSQL provides two tablespaces by default:
  + pg_default(store user data)
  + pg_global(store system data)

# Functions

+ Block of reusable SQL Code
+ Returns scalar value of a list of records
+ Returns composite objects

# Casts and Operators

+ Convert one data type into another data type
+ Used with functions to perform conversion
+ You can create custom casts to override the default
+ Operator is a symbolic function
+ You can define your own custom operators

# Sequences

+ Used to manage auto-increment columns


# Extensions

+ Used to wrap other objects into a single unit
+ Casts
+ Indexes
+ Functions
+ Etc

## built-in Extensions

+ adminpack
+ plpgsql

# Stored Procedures

+ A stored Procedures is a set of Structured Query Language(SQL) statements with an assigned name which are stored in a relational database management system as a group, so it can be reused and shared by multiple programs
+ PostgreSQL allows you to extend the database functionality with user-defined Functions
+ The store procedure define functions for creating triggers or custom aggregate functions

## Advantages of using PostgreSQL stored Procedures

+ Reduced network traffic
+ Increase application performance
+ Reusable

## Disadvantages of using PostgreSQL stored Procedures

+ Slow in software development
+ Make it idfficult to manage versions and hard to debug
+ Not portable to other database management system e.g, MySQL or Microsoft SQL Server
+ 

## Creating A Function

# Union operator


+ Used to combine result sets of multiple queries into a single result
+ Removes all duplicate rows
+ Both queries must return same number of columns
+ The corresponding columns in the queries must have compatible data types

```sql
select column1,column1
from table1
union
select column1,column1
from table2

```

# Union all operator

+ Used to combine result sets of multiple queries into a single result
+ Does not removes duplicate rows
+ Both queries must return same number of columns
+ The corresponding columns in the queries must have compatible data types

```sql
select column1,column1
from table1
union all
select column1,column1
from table2

```

# intersect operator

+ Used to combine result sets of two or more select statements into a single result
+ The INTERSECT operator returns all rows in both result sets
+ The number of columns that appear in the SELECT statement must be the same
+ The data types of the columns must be compatible


```sql
select column1,column1
from table1
intersect
select column1,column1
from table2;
```

# except operator

+ Returns rows by comparing the result sets of two or more queries
+ Returns rows in first query not present in output of the second query
+ Returns distinct rows from the first(left) query not in output of second(right) query
+ The number of columns and their orders must be the same in both queries
+ The data types of the columns must be compatible


```sql
select column1,column1
from table1
where conditionA
except
select column1,column1
from table2;
where conditionB

```

```sql
select film_id, title
from film
except
select distinct inventory.film_id, title
from inventory
         inner join film on film.film_id = inventory.film_id
order by title;

```
# IN operator

+ Specify or check against multiple values in a WHERE clause


```sql
select column_names 
from table_Name
where column_name in (value1, value2)

select column_names 
from table_Name
where column_name in (select)

```

# AND operator

+ Returns records matching all specified conditions
+ included in the where clause
+ Filters records based on more than one condition


```sql
select column_names
from table_Name
where condition1 AND condition2

```

# OR operator

+ Returns records matching any specified conditions
+ included in the where clause
+ Filters records based on more than one condition

```sql
select column_names
from table_Name
where condition1 OR condition2

```

# Combing The AND | OR Operators

+ AND operator and OR operator can be used in SELECT, INSERT, UPDATE, DELETE 
+ Use parethesis() when combing conditions


```sql
select *
from city
where (country_id = 91 and city = 'Basel')
   or city_id = 589

```

# LIKE operator

+ Used to query data by using a matching technique
+ Used with wildcards in the WHERE clause to search and match specified patterns
+ % wildcard allows you to match string of any length characters including zero
+ _ wildcard allows you to match a single character


```sql
select column1,column2
from table_Name
where column LIKE pattern

select first_name, last_name
from customer
where first_name like 'Jen%';

```
# INNER join

+ Used to retrieve data from multiple tables using INNER JOIN clause
+ Returns records that have matching values from columns in both tables

```sql
SELECT column_name(s)
FROM table_Name
INNER JOIN tableB on tableA.column_name = tableB.column_name;

select customer.customer_id, first_name, last_name, email, amount, payment_date
from customer
         inner join payment on payment.customer_id = customer.customer_id;

```
# LEFT JOIN 

+ Used to retrieve data from multiple tables using LEFT JOIN clause
+ Returns records from the left table and the matched records from the right table

```sql
SELECT column_name(s)
FROM table_Name
LEFT JOIN tableB on tableA.column_name = tableB.column_name;

select film.film_id, film.title, inventory_id from film
left join inventory on film.film_id = inventory.film_id
order by inventory_id nulls last

```

# FULL OUTER join


+ Used to retrieve data from multiple tables using FULL OUTER JOIN clause
+ Returns records from the left table and the right table
+ if rows in joined table do not match the full outer joins set NULL values
+ For the matching rows a single row is included in the result from both table
+ NOTE: FULL OUTER JOIN can potentially return very large result-sets!



```sql
SELECT column_name(s)
FROM table_Name
FULL OUTER JOIN tableB on tableA.column_name = tableB.column_name;

select film.film_id,
       film.title,
       inventory_id
from film
         full OUTER join inventory
                         on film.film_id = inventory.film_id
order by inventory_id asc nulls first ;

```

# CROSS JOIN


+ Used to retrieve data from multiple tables using CORSS JOIN clause

+ Returns records from the left table and the right table
+ returns records from joined tables
+ also called CARTESIAN JOIN(笛卡尔连接)
+ Produce the cartesian product of rows from combined joined tables
+ Does not require the tables to have matching columns in the JOIN clause


```sql
select * from table1 
cross join table2;

select * from table1, table2;

```

# NATURAL join

+ Used to retrieve data from multiple tables using NATURAL JOIN clause
+ Creates an implicit join based on matching column names in the joined tables
+ Natural join can be an INNER JOIN ,LEFT JOIN,RIGHT JOIN, 
+ PostgreSQL will use the INNER JOIN by default

```sql
SELECT * 
FROM table1 
NATURAL JOIN table2;


--下面三个查询的结果是一样的

select * from customer natural join payment;

select * from customer inner join  payment
using (customer_id);

select * from customer inner join  payment
on customer.customer_id = payment.customer_id

```

# Aggregate Functions

+ Aggregate functions perform a calculation on a set of rows and return a single row
+ Can be used in SELECT, HAVING and GROUP BY clauses

## Basic PostgreSQL Aggregate Functions 

+ avg() - return the average value
+ count() - return the number of values
+ max() - return the maximum value
+ min() - return the minimum value
+ sum() - return the sum of all of distinct values

## Avg()

+ Used to calculate the average value of a numeric column
+ Widely used
+ Can be used in SELECT, GROUP BY and HAVING clauses
+ ignores NULL values

```sql
avg(column)

select round(avg(replacement_cost), 2) avg_replacement_cost
from film


select customer.customer_id,
       first_name,
       last_name,
       to_char(avg(amount), '9999999999999.99') as average_amount
from payment
         inner join customer on payment.customer_id = customer.customer_id
group by customer.customer_id
order by average_amount desc


select customer.customer_id,
       first_name,
       last_name,
       to_char(avg(amount), '9999999999999.99') as average_amount
from payment
         inner join customer on payment.customer_id = customer.customer_id
group by customer.customer_id
having
    avg(amount) > 5
order by average_amount desc


```

## Count 

+ Returns the number of rows that match a specific condition of a query

```sql
select count(*) from table_name;


select count(*) drama_films
from film
inner join film_category using (film_id)
inner join category using(category_id)
where category_id = 7


select count (distinct amount) from payment

select customer_id,
       count(customer_id)
from payment
group by customer_id

select customer_id,
       count(customer_id)
from payment
group by customer_id
having count(customer_id) > 40;

```

## MAX()

+ Returns the maximum value in a set of values

```sql
MAX(expression)


select max(film.replacement_cost) from film;


select film_id, title
from film
where replacement_cost = (
    select max(replacement_cost)
    from film
    )
order by title

select *
from payment
where amount = (
    select
        max(amount)
    from payment
    )

select customer_id,
       max(amount)
from payment
group by customer_id


select customer_id,
       max(amount)
from payment
group by customer_id
having max(amount) > 10.99;


```



## MIN()

+ Returns the minimum value in a set of values

```sql
MIN(expression)


select min(film.replacement_cost) from film;


select film_id, title
from film
where replacement_cost = (
    select min(replacement_cost)
    from film
    )
order by title

select *
from payment
where amount = (
    select
        min(amount)
    from payment
    )

select customer_id,
       min(amount)
from payment
group by customer_id


select customer_id,
       min(amount)
from payment
group by customer_id
having min(amount) > 10.99;


```


## SUM()

+ Returns the sum of values or distinct values

```sql
SUM(distinct column)

select rating,
       sum(rental_duration)
from film
group by rating
order by rating

select
    customer_id,
    sum(amount) as total
from payment
group by customer_id
order by total desc
limit 5;


select
    customer_id,
    sum(amount) as total
from payment
group by customer_id
having sum(amount) > 200
order by total desc

```

# triggers

## What is a Trigger

+ A PostgreSQL trigger is a function invoked automatically whenever an event associated with a table occurs
+ Event could be INSERT,UPDATE, DELTE or TRUNCATE
+ A trigger is a special user-defined function that binds a table
+ You can specify whether the trigger is invoked before or after an event

## Types of Trigger

+ Row trigger
+ Statement trigger

## Benefits of using trigger

+ Maintain data integrity rules
+ Check activities of applications accessing the databaes

## Drawback of using triggers

+ Remember trigger exists
+ Understand it's logic


## PostgreSQL Trigger Features

+ PostgreSQL fires trigger for the TRUNCATE event
+ PostgreSQL allows you to define statement-level trigger on views
+ PostgreSQL requires you to define a user-defined function as the action of the trigger

## What you need to create PostgreSQL Trigger

+ Create a trigger function using CREATE FUNCTION statement
+ Bind this trigger function to a table using CREATE TRIGGER statement

## Creating a trigger function syntax

+ A trigger function is similar to an ordinary function, except that it does not tabke any arguments and has return value type trigger
+ Once the trigger function is defined, you can bind it to specific action on a table,

```sql
CREATE FUNCTION trigger_function() RETURN trigger AS

CREATE TRIGGER trigger_name{BEFORE|AFTER|INSTEAD OF}{event[OR...]}
ON table_name
[FOR[EACH]{ROW|STATEMENT}]
EXECUTE PROCEDURE trigger_function


```

```sql

CREATE TABLE employees
(
    id         serial PRIMARY KEY,
    first_name varchar(40) NOT NULL,
    last_name  varchar(40) NOT NULL
);

CREATE TABLE employee_audits
(
    id          serial PRIMARY KEY,
    employee_id int4         NOT NULL,
    last_name   varchar(40)  NOT NULL,
    changed_on  timestamp(6) NOT NULL
);



CREATE FUNCTION public.log_last_name_change()
    RETURNS trigger
    LANGUAGE 'plpgsql'
    COST 100
AS
$body$
BEGIN
    IF NEW.last_name <> OLD.last_name THEN
        INSERT INTO employee_audits(employee_id, last_name, changed_on)
        VALUES (OLD.id, OLD.last_name, NOW());
    END IF;
    RETURN new;
END
$body$;

ALTER TABLE public.log_last_name_change() OWNER TO postgres;


CREATE OR REPLACE TRIGGER last_name_changes
    BEFORE UPDATE
    ON employees
    FOR EACH ROW
EXECUTE PROCEDURE log_last_name_change();


```

## Managing PostgreSQL Trigger


```sql
ALTER trigger trigger_name on table_name
rename to new_name;

```

## MOdifying trigger

```sql

ALTER TABLE table_name
rename to new_name;


```

## Disabling trigger

```sql

ALTER TABLE table_name
DISABLE TRIGGER trigger_name | ALL;

```

## Removing trigger

```sql

drop trigger [if exists] trigger_name on table_name;

```

# Introduction to PostgreSQL Views

## What's a View?

+ A view is a basicially a stored query(SELECT statement)
+ A View is a vrtual table
+ A view is a Logical table representing data from underlying tables
+ A view is defined based on ore or more tables known as base tables
+ A view itself does not store data physically except for materializied views

## Benefits of Views

+ Simplifies complex queries
+ Can help hide sensitie table data
+ You can grant permission to users
+ View provides consistent layer


## Managing views

### creating

```sql
create view view_name as query;

create view customer_master as
SELECT cu.customer_id                                                 AS id,
       (((cu.first_name)::text || ' '::text) || (cu.last_name)::text) AS name,
       a.address,
       a.postal_code                                                  AS "zip code",
       a.phone,
       city.city,
       country.country,
       CASE
           WHEN cu.activebool THEN 'active'::text
           ELSE ''::text
           END                                                        AS notes,
       cu.store_id                                                    AS sid
FROM ((customer cu
    JOIN address a ON ((cu.address_id = a.address_id)))
    JOIN city ON ((a.city_id = city.city_id))
    JOIN country ON ((city.country_id = country.country_id)));


```

### modifying

```sql
CREATE OR REPLACE view_name 
AS query

alter view view_name rename to new_view_name;

```

### Removing

```sql
DROP VIEW [IF EXISTS] view_name

```

## PostgreSQL Updatable views

### conditions

+ The defining query of the view must have exactly one entry in the FROM clause
+ The defining query must not contain one of the following clauses at top level: GROUP BY, HAVING, LIMIT OFFSET, DISTINCT, WITH UNION, INTERSECT, and EXCEPT
+ The selection list must not contain any window function or set-returning function or any aggregate function such as SUM, COUNT, AVG, MIN, MAX, etc
+ An Updatable view may contain both updatable and non-updatable columns
+ CHECK OPTION
+ Permission on view

## Creating PostgreSQL Updatable Views

```sql
CREATE VIEW usa_cities AS
SELECT city,
       country_id
FROM city
WHERE country_id = 103;

SELECT * from usa_cities

insert into usa_cities (city, country_id) values ('San Jose', 103);
DELETE from usa_cities WHERE city = 'San Jose';
DELETE from usa_cities WHERE country_id = 104;


```


## Introduction To PostgreSQL materializied Views

### What are PostgreSQL materializied Views

+ PostgreSQL materializied views that allow you to store result of a query physically and update the data periodically
+ A materialized view caches the result of a complex expensive query and then allow you to refresh this result periodically(周期性)
+ The materializied views are useful in many cases that require fast data access
+ Used in data warehouses ob business intelligent applications

## Creating PostgreSQL materializied Views

```sql
CREATE MATERIALIZED VIEW view_name
AS QUERY WITH [NO] DATA;

REFRESH MATERIALIZED VIEW view_name;


-- avoid table lock
-- view must have a unique index
REFRESH MATERIALIZED VIEW CONCURRENTLY view_name;

DROP MATERIALIZED VIEW view_name;

CREATE MATERIALIZED VIEW rental_by_category
AS
SELECT c.name        AS category,
       SUM(p.amount) AS total_sales
FROM ((((
    (payment p JOIN rental r ON ((p.rental_id = r.rental_id)))
        JOIN inventory i ON ((r.inventory_id = i.inventory_id)))
    JOIN film f ON ((i.film_id = f.film_id)))
    JOIN film_category fc ON ((f.film_id = fc.film_id)))
    JOIN category c ON ((fc.category_id = c.category_id)))
GROUP BY c.name
ORDER BY SUM(p.amount) DESC
WITH NO DATA;

REFRESH MATERIALIZED VIEW rental_by_category;

SELECT *
FROM rental_by_category

CREATE UNIQUE INDEX rental_category ON rental_by_category (category)

REFRESH MATERIALIZED VIEW CONCURRENTLY rental_by_category;



```

# Analytic(Window) Functions

## What are Analytic(Window) Functions

+ Analytic functions compute an aggregate value based on a group of rows
+ Unlike aggregate functions,they can return multiple rows for each group


+ ROW_NUMBER()：为结果集中的每一行分配一个唯一的序号。
+ RANK() 和 DENSE_RANK()：根据指定的排序顺序为结果集中的每一行分配一个排名。RANK() 在遇到相同值时会留下排名间隙，而 DENSE_RANK() 则不会。
+ LEAD() 和 LAG()：查看当前行之后或之前的行的值。
+ SUM() OVER()：计算累计总和。
+ AVG() OVER()：计算移动平均数。
+ COUNT() OVER()：计算窗口内的行数。
+ FIRST_VALUE() 和 LAST_VALUE()：获取窗口内第一行或最后一行的值。

```sql
SELECT
  column1,
  column2,
  window_function([arguments]) OVER (
    [PARTITION BY expression1, ...]
    [ORDER BY expression2 [ASC | DESC], ...]
    [window_frame]
  )
FROM
  table_name;
```

## Types of Analytic(window) Functions

+ ROW_NUMBER
+ RANK
+ DENSE_RANK functions
+ FIRST_VALUE
+ LAST_VALUE functions
+ LAG and LEAD functions

## Using AVG as a Window Functions

```sql
CREATE TABLE product_groups
(
    group_id   serial PRIMARY KEY,
    group_name varchar(255) NOT NULL
);

CREATE TABLE products
(
    product_id   serial PRIMARY KEY,
    product_name varchar(255) NOT NULL,
    price        decimal(11, 2),
    group_id     int          NOT NULL,
    FOREIGN KEY (group_id) REFERENCES product_groups (group_id)
);

INSERT INTO product_groups (group_name)
VALUES ('Smartphone'),
       ('Laptop'),
       ('Tablet');

INSERT INTO products (product_name, group_id, price)
VALUES ('Microsoft Lumia', 1, 200),
       ('HTC One', 1, 400),
       ('Nexus', 1, 500),
       ('IPhone', 1, 900),
       ('HP ELite', 2, 1200),
       ('Lenovo Thinkpad', 2, 700),
       ('Sony VAIO', 2, 700),
       ('Dell Vostro', 2, 800),
       ('IPad', 3, 700),
       ('Kindle Fire', 3, 150),
       ('Sansung Glaxy Tab', 3, 200);

SELECT *
FROM products;
SELECT *
FROM product_groups;

SELECT AVG(products.price)
FROM products;

SELECT product_name,
       price,
       group_name,
       AVG(price) OVER (PARTITION BY group_name)
FROM products
         INNER JOIN product_groups USING (group_id);


```

## ROW_NUMBER() Functions

```sql
ROW_NUMBER() OVER (
    [PARTITION BY column_1, column_2...]
    [ORDER BY colum_3, column_4...]
)

SELECT product_name,
       price,
       group_name,
       row_number() OVER (PARTITION BY group_name ORDER BY price)
FROM products
         INNER JOIN product_groups USING (group_id);

SELECT product_name,
       price,
       group_name,
       row_number() OVER ()
FROM products
         INNER JOIN product_groups USING (group_id);

SELECT product_name,
       price,
       group_name,
       row_number() OVER (PARTITION BY group_id ORDER BY product_name)
FROM products
         INNER JOIN product_groups USING (group_id);

SELECT DISTINCT
       price,
       row_number() OVER (ORDER BY price)
FROM products
ORDER BY price

SELECT *
FROM products
WHERE price = (SELECT price
               FROM (SELECT price,
                            ROW_NUMBER() OVER (ORDER BY price DESC) nth
                     FROM (SELECT DISTINCT (price)
                           FROM products) prices) sorted_prices
               WHERE nth = 3)

```
## RANK() function

+ The RANK() function assigns ranking within an ordered partition



```sql
SELECT product_name,
       group_name,
       price,
       RANK() OVER (
           PARTITION BY group_name
           ORDER BY price
           )
FROM products
         INNER JOIN product_groups USING (group_id)


```


## DENSE_RANK() Function

+ The DENSE_RANK() function assigns ranking within an ordered partition, but the ranks are consecutive(连续的)

```sql

SELECT product_name,
       group_name,
       price,
       DENSE_RANK() OVER (
           PARTITION BY group_name
           ORDER BY price
           )
FROM products
         INNER JOIN product_groups USING (group_id)

```

## FIRST_VALUE() Function

+ The FIRST_VALUE() function returns the first value from the first row of the ordered set


```sql

SELECT product_name,
       group_name,
       price,
       FIRST_VALUE(price) OVER (
           PARTITION BY group_name
           ORDER BY
               price
           ) AS lowest_price_per_group,

       FIRST_VALUE(price) OVER (
           PARTITION BY group_name
           ORDER BY
               price
               DESC
           ) AS highst_price_per_group
FROM products
         INNER JOIN product_groups USING (group_id)

```


## LAST_VALUE() Function

+ The LAST_VALUE() function returns the last value from the first row of the ordered set

```sql
SELECT product_name,
       group_name,
       price,
       LAST_VALUE(price) OVER (
           PARTITION BY group_name
           ORDER BY
               price RANGE BETWEEN UNBOUNDED PRECEDING
               AND UNBOUNDED FOLLOWING
           ) AS highest_price_per_group
FROM products
         INNER JOIN product_groups USING (group_id)

SELECT product_name,
       group_name,
       price,
       LAST_VALUE(price) OVER (
           PARTITION BY group_name
           ORDER BY
               price
           ) AS highest_price_per_group
FROM products
         INNER JOIN product_groups USING (group_id)

```


## LAG() function

+ The LAG() function has the ability to access data from the previous row

```sql
LAG(expression[offset][default])

SELECT product_name,
       group_name,
       price,
       LAG(price, 1) OVER (
           PARTITION BY group_name
           ORDER BY
               price
           ) AS pre_price,
       price - LAG(price, 1) OVER (
           PARTITION BY group_name
           ORDER BY
               price
           ) AS cur_pre_diff
FROM products
         INNER JOIN product_groups USING (group_id)

```

## LEAD() Function

+ The LEAD() function has the ability to access data from the next row


```sql
LEAD(expression[offset][default])

SELECT product_name,
       group_name,
       price,
       LEAD( price, 1) OVER (
           PARTITION BY group_name
           ORDER BY
               price
           ) AS pre_price,
       price - LEAD(price, 1) OVER (
           PARTITION BY group_name
           ORDER BY
               price
           ) AS cur_next_diff
FROM products
         INNER JOIN product_groups USING (group_id);


```
