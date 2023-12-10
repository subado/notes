---
title: "1 Tutorial to PostgreSQL"
date: 2023-11-10T19:33:00+04:00
draft: true
---

## 1 Getting Started

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

The PostgreSQL server starts (“forks”) a new process for each connection
to handle multiple concurrent connections from clients.
The supervisor server process `postgres` is always running, waiting for client connections,
whereas clientand associated server processes come and go.

### Creating a Database

```bash
$ createdb DATABASE_NAME
```

This command should from user which controls `postgres` process or
you have to create a PostgreSQL user account for your OS user.

Also your user account should have the privileges required to create a database.

Database names must have an alphabetic first character and are limited to 63 bytes in length.

This action physically removes all files associated with the database and cannot be undone:

```bash
$ dropdb DATABASE_NAME
```

### Accessing a Database

`psql` is the PostgreSQL interactive terminal program, which allows you
to interactively enter, edit, and execute SQL commands.

```bash
$ psql DATABASE_NAME
```

`=#` instead of `=>` means that you are a database superuser.

The `psql` program has a number of internal commands which begin with the backslash character, '\'.
Type `\?` for more internal commands.

## 2 The SQL Language

### Concepts

**PostgreSQL** is a relational database management system (RDBMS).
It is a system for managing data stored in relations.

**Relation** is essentially a mathematical term for table.

Each table is a named collection of **rows**.
Each row of a given table has the same set of named **columns**,
and each column is of a specific data type.
Whereas columns have a fixed order in each row,
it is important to remember that SQL does not guarantee the order of the rows within the table in any
way (although they can be explicitly sorted for display).

Tables are grouped into databases, and a collection of databases managed by a single PostgreSQL
server instance constitutes a **database cluster**.

### Creating a new table

A `CREATE TABLE` is used to create base tables.

```sql
CREATE TABLE tablename (
        name1 type1,
        name2 type2,
        nameN typeN -- Hey, I'm a comment
);
```

The `;` means end of command. Two dashes (`--`) introduce comments.

SQL is case-insensitive about key words and identifiers, except when
identifiers are double-quoted to preserve the case.

PostgreSQL supports the standard SQL types `int`, `smallint`, `real`, `double precision`,
`char(N)`, `varchar(N)`, `date`, `time`, `timestamp`, and `interval`, as well as other types of
general utility and a rich set of geometric types.

PostgreSQL can be customized with an arbitrary number of user-defined data types.
Type names are not key words in the syntax.

A `DROP TABLE` is used to remove tables.

```sql
DROP TABLE babushka;
```

### Populating a Table With Rows

For this purpose you should use an `INSERT INTO`.

```sql
INSERT INTO words VALUES ('cat', 'angry animal that only poops your flat');
```

> In production you should explicitly specify column names.

```sql
INSERT INTO words (word, definition) VALUES ('cat', 'angry animal that only poops your flat');
```

Also you can use a `COPY` to copy data from flat-text files.

### Querying a Table

To retrieve data from a table, the table is **queried**.
A `SELECT` is used to do this.

The `SELECT` statement is divided into

- **a select list** (the part that lists the columns to be returned);
- **a table list** (the part that lists the tables from which to retrieve the data);
- an **optional qualification** (the part that specifies any restrictions);

```sql
SELECT label, price FROM drinks;
```

The `AS` clause is used to relabel the output column. The `AS` is optional.

```sql
SELECT (price/2) AS discount FROM drinks
```

The `WHERE` contains a Boolean (truth value) expression, and only rows for which the Boolean
expression is true are returned.
The usual Boolean operators (AND, OR, and NOT) are allowed in the
qualification.

```sql
SELECT * FROM drinks
    WHERE label = 'Vodka Rus Matushka' AND price < 100;
```

The `ORDER` means that results of a query be returned in sorted order.

```sql
SELECT * FROM drinks
    ORDER BY price;
```

The `DISTINCT` means that duplicate rows be removed from the result of a query.

```sql
SELECT DISTINCT label FROM drinks;
```

### Joins Between Tables

Queries that access multiple tables (or multiple instances of the same table)
at one time are called **join queries**(`JOIN`).
They combine rows from one table with rows from a second table, with an expression
specifying which rows are to be paired.

Qualification of the column names:

```sql
SELECT table.column FROM table;
```

```sql
SELECT * FROM drinks JOIN snaks ON snaks.price < drinks.price;
```

Without qualification code above will be ambiguous.

> It is widely considered good style to qualify all column names in a join query.

The code below works same as code with `JOIN`, but this code uses old implicit syntax.

```sql
SELECT * FROM drinks, snaks WHERE snaks.price < drinks.price;
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

```sql
SELECT DISTINCT d1.label, d1.price,
       d2.label, d2.price
FROM drinks d1 JOIN drinks d2
    ON d1.price > d2.price and d1.label != d2.label; -- Can help you, if drink's price from d1 is too much for you
```

Using `d1` and `d2` instead of drinks called **abbreviating**.

### Aggregate Functions

An **aggregate function** computes a single result from multiple input rows
(`count`, `min`, `avg`, `sum`, `max`).

```sql
SELECT min(price) FROM drinks;
```

The **subquery** is an independent computation that computes its own aggregate
separately from what is happening in the outer query.

```sql
SELECT label FROM drinks
    WHERE price = (SELECT max(price) FROM drinks);
```

In the above code subquery is necessary due to the fact, that the `WHERE`
clause determines which rows will be included in the aggregate calculation.

The `GROUP BY` clause groups rows by some condition,
and each aggregate result is computed over the table rows matching
that condition.

```sql
SELECT label, count(*), avg(price)
    FROM drinks
    GROUP BY label;
```

The `HAVING` clause selects group rows after groups and aggregates are computed.
While the `WHERE` selects input rows before groups and aggregates are computed.

The `FILETER` clause selects input rows for the input of the particular aggregate function,
that it is attached to.

```sql
SELECT label, count(*) FILTER (WHERE price < 50), min(price)
    FROM drinks
    GROUP BY label;
```

`GROUP BY`, `HAVING`, `LIKE` and `FILTER` are useful with aggregate functions.

### Updates

You can update existing rows using the `UPDATE` command.

```sql
UPDATE drinks
    SET price = price / 2
    WHERE label = 'Essa'; -- Everybody like discounts!
```

### Deletions

Rows can be removed from a table using the `DELETE` command.

```sql
DELETE FROM drinks WHERE expire < 'today'; -- Nothing is permanent(
```

```sql
DELETE FROM tablename;
```

Without a qualification, DELETE will remove all rows from the given table, leaving it empty.

## Advanced Features

### Views

A **view** gives a name to the query that you can refer to like an ordinary table(`CREATE VIEW`).

```sql
CREATE VIEW alternatives AS SELECT DISTINCT d1.label AS original,
       d2.label alternative, d2.price, (d1.price - d2.price) AS difference
FROM drinks d1 JOIN drinks d2
    ON d1.price > d2.price and d1.label != d2.label;
```

> Making liberal use of views is a key aspect of good SQL database design.

### Foreign Keys

A **foreig key** is a primary key of another table with use in this table as reference.
A **referential integrity** is a structure of tables in which
allowed values for some columns are listed in other table.

```sql
CREATE TABLE drinks (
    label text primary key,
    price smallint
);

CREATE TABLE dinners (
    drink text references drinks(label),
    meal text,
```

> Making correct use of foreign keys will definitely improve the quality of your database applications

### Transactions

A **transaction** bundles multiple steps(operations) into a single, all-or-nothing operation.
The intermediate states between the steps are not visible to other _concurrent_ transactions.

All the updates made by a transaction are logged in permanent storage
before the transaction is reported complete.

Syntax:

```sql
BEGIN;
<SQL QUERIES>
COMMIT; -- or ROLLBACK to undo changes
```

```sql
BEGIN;
INSERT INTO drinks VALUES ('Baltika', 33);
UPDATE drinks SET price = 44
    WHERE label = 'Essa';
COMMIT;
```

A group of statements surrounded by BEGIN and COMMIT is sometimes called a **transaction block**.

> PostgreSQL actually treats every SQL statement as being executed within a transaction.

**Savepoints**(`SAVEPOINT`) allow you to selectively discard parts of the transaction, while committing the
rest.
You can if needed roll back to the savepoint with `ROLLBACK TO`.

Releasing or rolling back to a savepoint will automatically
_release all savepoints_ that were defined after it

### Window Functions

A **window function** performs a calculation across a set of table rows
that are somehow related to the current row.
Window functions do not cause rows to become grouped into a single output row
like non-window aggregate calls would.
Instead, the rows retain their separate identities

Here is an example that shows how to compare each drink's price with
the average price of drink's with same expire date:

```sql
SELECT label, price, expire, avg(price)
    OVER (PARTITION BY expire) FROM drinks;
```

A window function call always contains an `OVER` clause
directly following the window function's name and argument(s).

The `OVER` clause determines exactly how the rows of the query
are split up for processing by the window function.
The `PARTITION BY` clause within `OVER` divides the rows into groups, or partitions,
that share the same values of the `PARTITION BY` expression(s).
For each row, the window function is computed across the rows that
fall into the same partition as the current row.

The `ORDER BY` within `OVER` controls the order in which rows are processed by window functions.

```sql
SELECT label, price, expire,
    rank() OVER (PARTITION BY expire ORDER BY price DESC)
FROM drinks;
```

For each row, there is a set of rows within its partition called its **window frame**.
if `ORDER BY` is supplied then the frame consists of all rows from the start of
the partition up through the current row, plus any following rows
that are equal to the current row according to the `ORDER BY` clause.

```sql
SELECT price, sum(price) OVER (ORDER BY price) FROM drinks;
```

Window functions are permitted only in the `SELECT` list and the `ORDER BY` clause of the query.

Window functions execute after non-window aggregate functions and `GROUP BY`, `HAVING` and `WHERE` clauses.
To deal with this you can use a sub-select.

A `WINDOW` clause is helpful if the same windowing behavior is wanted for several functions.

```sql
SELECT sum(price) OVER w, avg(price) OVER w
    FROM drinks
    WINDOW w AS (PARTITION BY expire ORDER BY price DESC);
```

### Inheritance

```sql
CREATE TABLE child_table (
  <columns declaration>
) INHERITS (parent_table);
```

A row of `child_table` inherits all columns from its parent.

```sql
CREATE TABLE definitions (
    style varchar(80),
    meaning text,
);

CREATE TABLE words (
    word varchar(45),
    part_of_speech varchar(45),
    forms text,
) INHERITS (definitions);

CREATE TABLE phrases (
    origin text,
) INHERITS (definitions);
```

The query which use parent table also applied to child tables.

```sql
SELECT style, meaning
    FROM definitions
    WHERE style = 'formal';
```

A `ONLY` allow you to run the query over only this table and exclude it's child tables.
