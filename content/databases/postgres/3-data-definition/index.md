---
title: "3 Data Definition"
date: 2023-12-09T21:01:04+04:00
draft: true
---

## 1 Table Basis

Tables consist:

1. rows

- number is variable,
- deal isn't specified.

2. columns

- number is fixed,
- deal is specified, when table is creating,
- have data types, which constrains the set of possible values
- have names.

To create table use:

```sql
CREATE TABLE my_first_table (
first_column text,
second_column integer
);
```

To delete table use:

```sql
DROP TABLE my_first_table;
```

> It is common in SQL script files to unconditionally
> try to drop each table before creating it, ignoring any error messages,
> so that the script works whether or not the table exists.

## 2 Default Values

A column can be assigned a default value.
When a new row is created and no values are specified
for some of the columns, those columns will be filled with their respective default values or null values.

```sql
CREATE TABLE drugs (
    name text,
    price numeric DEFAULT 55
);
```

A common examples are:

- for a timestamp column to have a default of `CURRENT_TIMESTAMP`,
- for a integer to generate number for each row

```sql
CREATE TABLE drugs (
drug_id integer DEFAULT nextval('drugs_drug_id_seq'),
...
);
```

## 3 Generated Columns

A **generated column** is a special column that is always computed from other columns.

Generated columns can be

1. **Stored**

   - occupies storage,
   - is computed when it is written (inserted or updated).

2. **Virtual**

   - occupies no storage
   - is computed when it is read

> PostgreSQL currently implements
> only stored generated columns.

To create a generated column, use the `GENERATED ALWAYS AS` clause in `CREATE TABLE`.
To choose the stored kind of generated column, use the `STORED` after the `GENERATED ALWAYS AS`.

```sql
CREATE TABLE people (
height_cm numeric,
height_in numeric GENERATED ALWAYS AS (height_cm / 2.54) STORED
);
```

Qualtiies of generated columns:

1. A generated column cannot be written to directly.

2. The keyword `DEFAULT` may be specified.

3. The generation expression can only use immutable functions and cannot use subqueries or reference
   anything other than the current row in any way.

4. A generation expression cannot reference another generated column.

5. A generation expression cannot reference a system column, except `tableoid`.

6. A generated column cannot have a column default or an identity definition.

7. A generated column cannot be part of a partition key.

8. Foreign tables can have generated columns.

9. For inheritance and partitioning:

   - If a parent column is a generated column, its child column must also be a generated column;
     however, the child column can have a different generation expression. The generation expression
     that is actually applied during insert or update of a row is the one associated with the table that
     the row is physically in. (This is unlike the behavior for column defaults: for those, the default
     value associated with the table named in the query applies.)

   - If a parent column is not a generated column, its child column must not be generated either.

   - For inherited tables, if you write a child column definition without any GENERATED clause in
     CREATE TABLE ... INHERITS, then its GENERATED clause will automatically be copied
     from the parent. ALTER TABLE ... INHERIT will insist that parent and child columns
     already match as to generation status, but it will not require their generation expressions to match.

   - Similarly for partitioned tables, if you write a child column definition without any GENERATED
     clause in CREATE TABLE ... PARTITION OF, then its GENERATED clause will auto-
     matically be copied from the parent. ALTER TABLE ... ATTACH PARTITION will insist
     that parent and child columns already match as to generation status, but it will not require their
     generation expressions to match.

   - In case of multiple inheritance, if one parent column is a generated column, then all parent
     columns must be generated columns. If they do not all have the same generation expression, then
     the desired expression for the child must be specified explicitly.

10. Generated columns maintain access privileges separately from their underlying base columns. So,
    it is possible to arrange it so that a particular role can read from a generated column but not from
    the underlying base columns.

11. Generated columns are, conceptually, updated after BEFORE triggers have run. Therefore, changes
    made to base columns in a BEFORE trigger will be reflected in generated columns. But conversely,
    it is not allowed to access generated columns in BEFORE triggers.

## 4 Constraints

**Constraints** give you as much control over the data in your tables as you wish.
SQL allows you to define constraints on columns and tables.

### Check Constraints

A **check constraint**(`CHECK`) allows you to specify that the value in
a certain column or several columns must satisfy a Boolean expression.

```sql
CREATE TABLE chess_gms (
    name text,
    rank numeric CHECK (rank > 2000)
);
```

The constraint above, called **column constraint**.

The constraint can have a separate name.
This clarifies error messages and allows you to refer
to the constraint when you need to change it.
This can be done using the `CONSTRAINT` keyword.
Otherwise postgres will generate name for your constraint automatically.

You can write constraint that is not attached to a particular column,
instead it appears as a separate item in the comma-separated column list.
This kind of constraints called **table constraints**,
because they attached to the table not to the particular column.

```sql
CREATE TABLE drugs (
    drug_id integer,
    name text,
    price numeric CHECK (price > 0),
    discounted_price numeric CHECK (discounted_price > 0),
    CONSTRAINT meaningful_discounted_price CHECK (price > discounted_price)
);
```

> A check constraint is satisfied if the check expression evaluates to true or the null value.

### Not-Null Constraints

A **not-null constraint** is a column constraint specifies that a column must not assume the null value.

```sql
CREATE TABLE drinks (
    label text, NOT NULL,
    price numeric NOT NULL
);
```

The `NOT NULL` constraint has an inverse: the `NULL` constraint which selects
the default behavior that the column might be null.

> The `NULL` constraint is not present in the SQL standard.

> In most database designs the majority of columns should be marked not null.

`NOT NULL` constraints do not have names

### Unique Constraints

**Unique constraints** ensure that the data contained in a column, or a group of columns,
is unique among all the rows in the table.

```sql
CREATE TABLE drinks (
    id integer UNIQUE,
    label text,
    price numeric
);
```

or

```sql
CREATE TABLE drinks (
    id integer,
    label text,
    price numeric,
    UNIQUE (id)
);
```

To span several columns:

```sql
CREATE TABLE drinks (
    id integer,
    label text,
    price numeric,
    UNIQUE (id, label)
);
```

But in the example above, the combination of id and label will be unique
instead of id and label be unique separately.

Adding a unique constraint will automatically create a unique B-tree index on the column or group of
columns listed in the constraint.

By default, two null values are not considered
equal in comparison of `UNIQUE` columns.
This behavior can be changed by adding the clause `NULLS NOT DISTINCT`.

```sql
CREATE TABLE drinks (
    id integer,
    label text,
    price numeric,
    UNIQUE NULLS NOT DISTINCT (id)
);
```

### Primary Keys

A **primary key constraint** indicates that a column, or group of columns, can be used as
a unique identifier for rows in the table.
This requires that the values be both unique and not null.

```sql
CREATE TABLE drinks (
    id integer PRIMARY KEY,
    label text,
    price numeric
);
```

The `PRIMARY KEY` supports same syntax as the `UNIQUE`

A table can have at most one primary key.
Relational database theory dictates that every table must have a primary key.

### Foreign Keys

A **foreign key constraint** specifies that the values in a column (or a group of columns)
must match the values appearing in some row of another table.

We say this maintains the **referential integrity** between two related tables.

```sql
CREATE TABLE drugs (                -- referenced table
    drug_id integer PRIMARY KEY,
    name text,
    price numeric
);

CREATE TABLE deals (                -- referencing table
    deal_id integer PRIMARY KEY,
    drug_id integer REFERENCES drugs (drug_id),
    /*
    The `drug_id integer REFERENCES drugs,`
    would be the same, because drug_id is the primary key of the drugs.
    */
    weight numeric
);
```

> It is impossible to create rows of referencing table with non-NULL
> referencing columns entries that do not appear in the referenced table.

```sql
CREATE TABLE t1 (
    a integer PRIMARY KEY,
    b integer,
    c integer,
    FOREIGN KEY (b, c) REFERENCES other_table (c1, c2) -- a group of columns
);
```

If a foreign key constraint to be the same table, then
it is called a **self-referential** foreign key
(Could be useful for graphs or tree structures).

```sql
CREATE TABLE tree (
    node_id integer PRIMARY KEY,
    parent_id integer REFERENCES tree,
    name text,
    ...
);
```

A table can have more than one foreign key constraint.
This is used to implement many-to-many relationships between tables.

```sql
CREATE TABLE drugs (
    drug_id integer PRIMARY KEY,
    name text,
    price numeric
);

CREATE TABLE deals (
    deal_id integer PRIMARY KEY,
    drop_address text,
...
);

CREATE TABLE deal_items (
    drug_id integer REFERENCES drugs,
    deal_id integer REFERENCES deals,
    quantity integer,
    PRIMARY KEY (drug_id, deal_id)
);
```

---

#### Handling deletion of referenced table rows

You need to use the `ON DELETE` command.

```sql
CREATE TABLE t1 (
    a integer REFERENCES t2 ON DELETE NO ACTION
);
```

`ON DELETE` has several actions:

1. `NO ACTION`

   - if any referencing rows still exist when the constraint is
     checked, an error is raised.
   - allows the check to be deferred until later
     in the transaction

2. `RESTRICT`

   - prevents deletion of a referenced row.
   - He isn't allowing the check to be deferred until later
     in the transaction.

3. `CASCADE`

   - deletes all referencing rows with referenced row.

4. `SET NULL`

   - set referencing column to null.

5. `SET DEFAULT`

   - set referencing column to default value.

The actions `SET NULL` and `SET DEFAULT` can take a column list to specify which columns to set.

#### Handling updating of referenced table rows

You need to use the `ON UPDATE` command.
The possible actions are the same with `ON DELETE`, except that column lists
cannot be specified for `SET NULL` and `SET DEFAULT`.

---

Normally, a referencing row need not satisfy the foreign
key constraint if any of its referencing columns are null.

If `MATCH FULL` is added to the foreign key declaration, a referencing row escapes
satisfying the constraint only if all its referencing columns are null

If you don't want referencing rows to be able to avoid satisfying the foreign key
constraint, declare the referencing column(s) as `NOT NULL`.

---

A foreign key must reference columns that either are a primary key or form a unique constraint.
This means that the referenced columns always have an index.

> It is often a good idea to index the referencing columns too.

### Exclusion Constraints

**Exclusion constraints** ensure that if any two rows are compared on the specified columns or expressions
using the specified operators, at least one of these operator comparisons will return false or null.

```sql
CREATE TABLE circles (
    c circle,
    EXCLUDE USING gist (c WITH &&)
);
```

Adding an exclusion constraint will automatically create an
index of the type specified in the constraint declaration.

## 5 System Columns

Every table has several system columns that are implicitly defined by the system.
These names cannot be used as names of user-defined columns.

- `tableoid`.

  The OID(object identifier) of the table containing this row.

- `xmin`

  The identity (transaction ID) of the inserting transaction for this row version.

  A **row version** is an individual state of a row;
  each update of a row creates a new row version for the same logical row.

- `cmin`

  The command identifier (starting at zero) within the inserting transaction.

- `xmax`

  The identity (transaction ID) of the deleting transaction, or zero for an undeleted row version.

- `cmax`

  The command identifier within the deleting transaction, or zero.

- `ctid`
  The physical location of the row version within its table.

Transaction identifiers and command identifiers are 32-bit quantities.

## 6 Modifying Tables

PostgreSQL provides a family of commands to alter the definition, or structure, of the table.

### Adding a Column

```sql
ALTER TABLE name ADD [ COLUMN ] [ IF NOT EXISTS ] column_name data_type [ COLLATE collation ] [ column_constraint [ ... ] ];
```

If a column with a constant default value added, then
the default value will be returned the next time the row is accessed.

However, if the default value is volatile (e.g., `clock_timestamp()`) each row will need
to be updated with the value calculated at the time `ALTER TABLE` is executed.

In this case better to add column with no default,
to do `UPDATE` with the correct values for columns which shouldn't have default values,
and to add default value to column.

### Removing a Column

```sql
ALTER TABLE name DROP [ COLUMN ] [ IF EXISTS ] column_name [ RESTRICT | CASCADE ];
```

Table constraints involving the column are dropped, too.
However, if the column is referenced by a foreign key constraint of another table, PostgreSQL will
not silently drop that constraint, and you need to use `CASCADE` in this case.

### Adding a Constraint

```sql
ALTER TABLE name ADD table_constraint [ NOT VALID ];
```

To add a not-null constraint, which cannot be written as a table constraint, use this syntax:

```sql
ALTER TABLE name ALTER COLUMN column SET NOT NULL;
```

### Removing a Constraint

To remove a constraint you need to know its name.
If you specified constraint name, then it is a easy task.
Otherwise, to find out a constraint generated name assigned by the system,
you can use the psql command `\d tablename`.

```sql
ALTER TABLE tablename DROP CONSTRAINT some_name;
```

To drop a not null constraint use:

```sql
ALTER TABLE tablename ALTER COLUMN some_name DROP NOT NULL;
```

### Changing a Column's Default Value

```sql
ALTER TABLE tablename ALTER COLUMN some_name SET DEFAULT some_value;
```

It just changes the default for future `INSERT` commands.

To remove any default value, use:

```sql
ALTER TABLE tablename ALTER COLUMN some_name DROP DEFAULT;
```

It is not an error to drop a default where one hadn't been defined,
because the default is implicitly the null value.

### Changing a Column's Data Type

```sql
ALTER TABLE tablename ALTER COLUMN some_name TYPE some_type;
```

This will succeed only if each existing entry in the column
can be converted to the new type by an implicit cast.

If a more complex conversion is needed, you can add a `USING` clause that
specifies how to compute the new values from the old.

### Renaming a Column

```sql
ALTER TABLE tablename RENAME [ COLUMN ] column_name TO new_column_name;
```

### Renaming a Table

```sql
ALTER TABLE tablename RENAME TO new_name;
```

## 7 Privileges
