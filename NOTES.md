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
