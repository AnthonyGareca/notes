# SQL

[back](../README.md)

## Data Query Language (DQL)

### SQL Components

Structure Query Languages

- Data Definition Language (DDL) - CREATE, DROP, ALTER
- Date Query Language (DQL) - SELECT
- Data Manipulation Language (DML) - SELECT, INSERT, UPDATE, DELETE
- Data Control Language (DCL) - GRANT, REVOKE
- Transaction Control Language (TCL) - COMMIT, ROLLBACK

### Data Query Language - DQL

A Data Query Language (DQL) is used to retrieve data from database and mostly contains SELECT statement.

It has two components:

- Retrieve Column
- Retrieve Row

> Query only as much data as your require

The purpose of database is to make sure that we retrieve data efficiently and effectively.

DQL components are very important for any RDBMS to retrieve data from the tables.

```sql
SELECT *
FROM rental;
/* Affected rows: 0  Found rows: 16,044  Warnings: 0  Duration for 1 query: 0.000 sec. (+ 0.031 sec. network) */

SELECT *
FROM actor
INNER JOIN film_actor ON film_actor.actor_id = actor.actor_id
INNER JOIN film ON film_actor.film_id = film.film_id;
/* Affected rows: 0  Found rows: 5,462  Warnings: 0  Duration for 1 query: 0.015 sec. (+ 0.031 sec. network) */
```

In queries that retrieve a lot of rows, you will see the `(+ 0.016 sec. network)` detail in the output. That means that even if your query is very fast and MariaBD retrieves the data very, very quickly, to display all the rows you requested, it took around 0.016 sec. so think about business setup or production server, you might have thousands of queries running from hundreds of different clients, at this point of time, very millisecond counts. And eventually you do concurrently running query, your system might get very slow. This is the reason it is not recommended that we retrieve all the data all the time from your table.

Also, think about the situation. When you are updating the table, your SELECT may not be able to access the same data, and eventually you'll get a very poor performance.

> DQL language components are very important for any RDBMS to retrieve data from the tables.

## Why avoid SELECT \*

- **Increases unnecessary IO for disk**. SELECT \* retrieves pretty much every single column from your table.
- **Increases unwarranted network traffic**. When our table is very wide and we only need few columns from the table in our application, pretty much we are abusing our IO and network with a lot more data than we need.
- **Requires more memory for data cache**. Every single query eventually goes to your memory pool buffer. Now we are retrieving every single time all the data. It means we need now more memory to handle that cache. If we do not more memory, every soon we start having a memory pressure.
- **Index inefficiency**. If you are retrieving every single column every single time from your table, SQL server will end up using your cluster index to retrieve data pretty much every time. Indexed are usually created on few columns of the table. When we are retrieving only those columns, indexes are beneficial and helpful, but if we are retrieving every single column, indexes may not be helpful at all. MariaDB Engine will end up retrieving all the columns from your cluster index or heap and will ignore your indexes.
- **Column binding for issues for column dependent applications**. It es quite possible that data architect may go and change the names of the table in MariaDB database. That would immediately break your code if your application is binding columns. SELECT \* does not give you any guarantee to retrieve data in the same order every single time in terms of columns.

In a real world scenario:

Table:

| ID  | FirstName | LastName | ManagerID | Salary |
| --- | --------- | -------- | --------- | ------ |

where we have a table with sensitive information about the employees and we want only retrieve the full name of our employees.

```sql
SELECT CONCAT(FirstName, ' ', LastName) AS FullName
WHERE Employee;
```

Now, we want to retrieve name of the employee and his or her manager.

```sql
SELECT CONCAT(e1.FirstName, ' ', e1.LastName) AS [EmployeeName],
    CONCAT(e2.FirstName, ' ', e2.LastName) AS [ManagerName]
FROM Employee e1
INNER JOIN Employee e2 ON e1.ManagerID = e2.ID
ORDER BY e1.ID;
```

- INNER JOIN, where it is joining Table 1's ManagerID with Table 2's ID. This is very critical information in self join when it want to find hierarchy of the employees.
- SELECT statement, the first where it is looking at the employee name, it is retrieving data from e1, which stands for employee 1, and when we are finding name of the manager, it is retrieving data from e2, which is employee table with alias e2.

Now we want to retrieve the actors that had work together in the most quantity of films possible:

```sql
-- to no repeat actors from a join to another we use less than mark (<) to prevent that behavior.
SELECT fa1.actor_id Actor1, CONCAT (a1.first_name, ' ', a1.last_name) Actor1Name,
       fa2.actor_id Actor2, CONCAT (a2.first_name, ' ', a2.last_name) Actor2Name,
       COUNT(fa1.film_id) TotalCount
FROM film_actor fa1
INNER JOIN film_actor fa2 ON fa1.film_id = fa2.film_id
           AND fa1.actor_id < fa2.actor_id
INNER JOIN actor a1 ON fa1.actor_id = a1.actor_id
INNER JOIN actor a2 ON fa2.actor_id = a2.actor_id
GROUP BY fa1.actor_id, fa2.actor_id
ORDER BY TotalCount DESC, fa1.actor_id, fa2.actor_id;
```

> Avoid using `SELECT *` in a table queries.

> Query only the columns that you need for optimal performance.

## Filter Table Rows

Golden rule: `Query only as much data as much is required by our application.`

- Limit results with `WHERE` clause
- Search for patterns with `LIKE` keyword
- Select a range of values with `BETWEEN` keyword
- Select a range of known values and not very big with `IN` keyword

Examples:

Table;

| ID  | FirstName | LastName | ManagerID | Salary |
| --- | --------- | -------- | --------- | ------ |

List employees called `Mary`.

```sql
SELECT CONCAT(FirstName, ' ', LastName) AS FullName, Salary, ManagerID
FROM Employee
WHERE FirstName = 'Mary';
```

List employees with first initial as 'M' or 'T'.

The `LIKE` operator is used in a `WHERE` clause to search for a specified pattern in a column.

There are two wildcards often used in conjunction with the `LIKE` operator:

- The percent sign (%) represents zero, one, or multiple characters (like \* in RegEx)
- The underscore (\_) represents one, single character

```sql
SELECT CONCAT(FirstName, ' ', LastName) AS FullName, Salary, ManagerID
FROM Employee
WHERE FirstName LIKE 'M%' OR FirstName LIKE 'T%';
```

List employees with salary between 5k and 9k.

The `BETWEEN` keyword includes both the ranges specified in the statement.

```sql
SELECT CONCAT(FirstName, ' ', LastName) AS FullName, Salary, ManagerID
FROM Employee
WHERE Salary BETWEEN 5000 AND 9000;
```

List employees with ManagerID 1, 3, and 5.

```sql
SELECT CONCAT(FirstName, ' ', LastName) AS FullName, Salary, ManagerID
FROM Employee
WHERE ManagerID IN (1, 3, 5);
```

Limit the list of employees that have salary more than 8K to 3.

```sql
SELECT CONCAT(FirstName, ' ', LastName) AS FullName, Salary, ManagerID
FROM Employee
WHERE Salary > 8000
LIMIT 3;
```

> Avoid retrieving entire table in your application code.

> Retrieves only the rows that you need for optimal performance.

## Aggregating Results with GROUP BY

A `GROUP BY` clause in a `SELECT` statement groups rows together that have the same value in one or more columns.

`GROUP BY` can be only used with `SELECT` expression. If you don't have `SELECT` expression, there is no meaning to use `GROUP BY`.

If you use `GROUP BY` clause you can have multiple expressions in it. You can separate multiple expressions by comma.

You don't only have to specify column name as expression. You can also use single integer as a grouping expression. If you use `2` it indicates that we want to group by second column, witch we have used in a `SELECT` expression.

If we use `WHERE` condition where `GROUP BY` clause is used, it is usually applied before grouping.

You don't have to always specify order by. `GROUP BY` clause automatically orders or sorts rows based on expression specified in `GROUP BY` clause. The default sort order is ascending.

`HAVING` clause for helps us filter with data based on aggregation functions, witch we have use in `SELECT` statement.

```sql
-- Example
SELECT Col1, Col2, AVG(Col3)
FROM TableName
WHERE Col1 = 'Condition'
GROUP BY Col1, 2
HAVING AVG(Col3) > 100;
```

### Common Aggregated Functions

- `AVERAGE` calculates the average of group of selected value in a particular column.
- `SUM` adds together all the values in a particular column and returns us a single value.

- `COUNT` counts how many rows are in a particular column. You can specify star (\*) or just column name in this function, and it counts appropriated value.
- `MAX` return the highest value in a particular column.
- `MIN` return the lowest value in a particular column.
- `COUNT DISTINCT`, it actually counts distinct value in you particular column.

Examples:

Find the average rental amount paid by each customer.

```sql
-- Using Sakila DB
SELECT AVG(amount) AVGAmount, customer_id
FROM payment
GROUP BY customer_id;
```

List customers who paid average rental amount over USD 5

```sql
-- Using Sakila DB
SELECT AVG(amount) AVGAmount, customer_id
FROM payment
GROUP BY customer_id
HAVING AVG(amount) > 5;
```

Find the total rental amount paid by each customer

```sql
SELECT SUM(amount) TotalAmount, customer_id
FROM payment
GROUP BY customer_id
ORDER BY TotalAmount DESC;
```

Find the maximum amount paid by any customer whish in not complimentary (not use coupons or any other discount in a rental to pay zero (0))

```sql
SELECT MAX(amount) MAXAmount, MIN(amount) MINAmount
FROM payment
WHERE amount > 0;
```

Find count of customer based on the amount paid

Using `WITH ROLLUP` (super-aggregate summaries) we can see the total of the `COUNT` aggregated function at the end of the rows. It cannot be used with `ORDER BY`, some sorting is still possible by using `ASC` or `DESC` clause with the `GROUP BY` column, although the super-aggregated rows will always added last.

**Be Careful**: if you only use the COUNT(customer_id) you will count the number of payments done per each amount, don't count the number of customers at all.

```sql
SELECT COUNT(DISTINCT customer_id) TotalCustomer, amount
FROM payment
GROUP BY amount
WITH ROLLUP;
```

> `GROUP BY` clause groups rows together in `SELECT` statement based on Grouping Function.

> MariaDB: Aggregated Functions, most popular are AVG, SUM, COUNT, MAX, MIN, and COUNT DISTINCT. The rest are [here](https://mariadb.com/kb/en/aggregate-functions/).

[back](../README.md)
