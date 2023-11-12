---
title: "Inroduction to PostgreSQL"
date: 2023-11-10T19:33:00+04:00
draft: true
---

## Tutorial

### Conventions

The following conventions are used in the synopsis of a command:

- Brackets ([ and ]) indicate optional parts.
- Braces ({ and }) and vertical lines (|) indicate that you must choose one alternative.
- Dots (...) mean that the preceding element can be repeated.

All other symbols, including parentheses, should be taken literally.
Where it enhances the clarity, SQL commands are preceded by the prompt =>, and shell commands
are preceded by the prompt $. Normally, prompts are not shown, though.

### Architectural Fundamentals

PostreSQL uses a client/server model which consists of two parts:

- A server process, which manages the database files, accepts connections to the database from client
  applications, and performs database actions on behalf of the clients. The database server program
  is called `postgres`.
- The user's client (frontend) application that wants to perform database operations.

The PostgreSQL server starts (“forks”) a new process for each connection to handle multiple concurrent connections from clients.

### Accessing a Database

The psql program has a number of internal commands which begin with the backslash character, "\".

## The SQL Language

### Concepts

**PostgreSQL** is a relational database management system (RDBMS).
**Relation** is essentially a mathematical term for table.

Tables are grouped into databases, and a collection of databases managed by a single PostgreSQL
server instance constitutes a database cluster.

### Creating a new table

A `CREATE TABLE` is used to create base tables.

SQL is case-insensitive about key words and identifiers, except when identifiers are double-quoted to preserve the case.

PostgreSQL supports the standard SQL types int, smallint, real, double precision,
char(N), varchar(N), date, time, timestamp, and interval, as well as other types of
general utility and a rich set of geometric types.

A `DROP TABLE` is used to remove tables.

### Populating a Table With Rows

For this purpose you should use an `INSERT`.

Also you can use a `COPY` to copy data from flat-text files.

### Querying a Table

To retrieve data from a table, the table is **queried**.

A `SELECT` is used to do this.
A `WHERE` is useful to filter table data.
An `AS` may you relabel the output column.
An `ORDER` helps you to sort the output.
A `DISTICT` delete duplicates from the output.

### Joins Between Tables

Queries that access multiple tables (or multiple instances of the same table)
at one time are called **join queries**(`JOIN`).

Qualification of the column names:

```sql
SELECT table.object FROM table;
```

Qualified joins:

```sql
T1 { [INNER] | { LEFT | RIGHT | FULL } [OUTER] } JOIN T2 ON boolean_expression
T1 { [INNER] | { LEFT | RIGHT | FULL } [OUTER] } JOIN T2 USING ( join column list )
T1 NATURAL { [INNER] | { LEFT | RIGHT | FULL } [OUTER] } JOIN T2
```

Join types:

- `INNER JOIN`
  For each row R1 of T1, the joined table has a row for each row in T2 that satisfies the join condition with R1.
- `LEFT OUTER JOIN`
  First, an inner join is performed. Then, for each row in T1 that does not satisfy the join condition with any row in T2,
  a joined row is added with null values in columns of T2. Thus, the joined table always has at least one row for each row in T1.
- `RIGHT OUTER JOIN`
  First, an inner join is performed. Then, for each row in T2 that does not satisfy the join condition with any row in T1,
  a joined row is added with null values in columns of T1. This is the converse of a left join:
  the result table will always have a row for each row in T2.
- `FULL OUTER JOIN`
  First, an inner join is performed. Then, for each row in T1 that does not satisfy the join condition with any row in T2,
  a joined row is added with null values in columns of T2.
  Also, for each row of T2 that does not satisfy the join condition with any row in T1,
  a joined row with null values in the columns of T1 is added.

A a **self join** is a process of joining a table against itself.

### Aggregate Functions

An **aggregate function** computes a single result from multiple input rows.
The **subquery** is an independent computation that computes its own aggregate
separately from what is happening in the outer query.

`HAVING`, `LIKE` and `FILTER` commands are useful with aggregate functions.

### Updates

You can update existing rows using the `UPDATE` command.

### Deletions

Rows can be removed from a table using the `DELETE` command.

## Advanced Features

### Views

A **view** gives a name to the query that you can refer to like an ordinary table(`CREATE VIEW`).

### Foreign Keys

A **foreig key** is a primary key of another table with use in this table as reference.
A **referential integrity** is a structure of tables in which allowed values for come collums are listed in other table.

### Transactions

A **transaction** is a mechanism which guarantee that our changes of database will be safety and can not corrupt database.
Transaction are executed _concurrently_.

Syntax:

```sql
BEGIN
<SQL QUERIES>
COMMIT -- or ROLLBACK to undo changes
```

A group of statements surrounded by BEGIN and COMMIT is sometimes
called a **transaction block**.

Savepoints(`SAVEPOINT`) allow you to selectively discard parts of the transaction, while committing the
rest.
You can if needed roll back to the savepoint with `ROLLBACK TO`.

### Window Functions

A **window function** performs a calculation across a set of table rows that are somehow related to the current row.

A window function call always contains an `OVER` clause.

For each row, there is a set of rows within its partition called its **window frame**.

Window functions are permitted only in the `SELECT` list and the `ORDER BY` clause of the query.

A `WINDOW` clause is helpful if the same windowing behavior is wanted for several functions.

### Inheritance

```sql
CREATE TABLE child_table (
  <columns declaration>
) INHERITS (parent_table);
```

A row of `child_table` inherits all columns from its parent.

A `ONLY` allow you to run the query over only this table and exclude it's child tables.
