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

### Components of DQL

## Specific Column Data

## Specific Row Data

## Summary

[back](../README.md)
