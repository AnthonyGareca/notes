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

[back](../README.md)
