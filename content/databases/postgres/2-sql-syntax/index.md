---
title: "2 SQL syntax"
date: 2023-12-09T10:15:05+04:00
draft: true
---

## 1 Lexical Structure

SQL input consists of a sequence of _commands_.
A **command** is composed of a sequence of tokens, terminated by a semicolon (“;”).
The end of the input stream also terminates a command.
Which tokens are valid depends on the syntax of the particular command.
A **token** can be a key word, an identifier, a quoted identifier, a literal (or constant), or a special character
symbol.

> If a special character is adjacent to some other token type then
> tokens should be separated by nwhitespace (space, tab, newline).

```sql
SELECT price FROM beers; -- I'm correct
```

**Comments** are effectively equivalent to whitespace.

### Identifiers and Key Words

**Key words** are words that have a fixed meaning in the SQL language.(`SELECT`)

**Identifiers** are names that identify names of tables, columns, or
other database objects, depending on the command they are used in.

SQL identifiers and key words must begin with

- a letter,
- an underscore (\_).

Subsequent characters in an identifier or key word can be

- letters,
- underscores,
- digits (0-9),
- dollar signs ($).

> Dollar signs are not allowed in identifiers
> according to the letter of the SQL standard

The SQL standard will not define a key word that contains digits or starts or ends with an underscore.

The system uses no more than `NAMEDATALEN`-1 (63 by default) bytes of an identifier.

> Key words and unquoted identifiers are case-insensitive

The **delimited identifier or quoted identifier** is a sequence of any characters,
except the character with code zero, enclosed in double-quotes (").
A delimited identifier is always an identifier, never a key word.

```sql
select "select" from "selections"; -- There "select" is a column name
```

> Quoting an identifier also makes it case-sensitive, whereas unquoted names are always folded to lower case,
> the SQL standard says that unquoted names should be folded to upper case.

A variant of quoted identifiers allows including escaped Unicode characters identified by their code
points.

This kind of quoted string should started with `U&`.

Code points are written using a escape character and

- four-digit hexadecimal,
- plus sign with six-digit hexadecimal.

The default escape character is `\`.
It can be changed using `UESCAPE`.

```sql
U&"d!0061t!+000061" UESCAPE '!'
```

### Constants

A **string constant** in SQL is an arbitrary sequence of characters bounded by single quotes (')

> Two string constants that are only separated by whitespace with at least
> one newline are concatenated and effectively treated as
> if the string had been written as one constant.

```sql
SELECT 'Kaneki'
       'Ken';
```

The Postgres allows you to write string constants with C-style escapes using `E`(`e`)
leter before the first single quote.

| Backslash Escape Sequence         | Interpretation                                   |
| --------------------------------- | ------------------------------------------------ |
| \b                                | backspace                                        |
| \f                                | form feed                                        |
| \n                                | newline                                          |
| \r                                | carriage return                                  |
| \t                                | tab                                              |
| \o, \oo, \ooo (o = 0–7)           | octal byte value                                 |
| \xh, \xhh (h = 0–9, A–F)          | hexadecimal byte value                           |
| \uxxxx, \Uxxxxxxxx (x = 0–9, A–F) | 16 or 32-bit hexadecimal Unicode character value |

```sql
SELECT E'\\aloha\tdance';
```

A **dollar-quoted string** constant consists of a dollar sign ($), an
optional “tag” of zero or more characters, another dollar sign, an arbitrary sequence of characters that
makes up the string content, a dollar sign, the same tag that began this dollar quote, and a dollar sign.

```sql
SELECT $sheesh$Try to outpu\t this\\\BIG$Seeesh$sheesh$;
```

**Bit-string** constants look like regular string constants with a `B`(`b`) or `X`(`x`) immediately
before the opening quote (no intervening whitespace).

```sql
SELECT X'F';
SELECT B'1111';
```

**Numeric constants** are accepted in these general forms:

```
digits -- used for decimal
digits.[digits][e[+-]digits]
[digits].digits[e[+-]digits]
digitse[+-]digits
```

```
0xhexdigits - use for other
0ooctdigits
0bbindigits
```

For visual grouping, underscores can be inserted between digits.

```
0xFFFF_FFFF
```

#### Constants of Other Types

A constant of an _arbitrary_ type can be entered using any one of the following notations:

```sql
type 'string'
type ( 'string' )
'string'::type
CAST ( 'string' AS type )
```

### Operators

An **operator name** is a sequence of up to `NAMEDATALEN`-1 (63 by default)
characters from the following list:

```
+-*/<>=~!@#%^&|`?
```

There are a few restrictions on operator names, however:

- -- and /\* cannot appear anywhere in an operator name, since they will be taken as the start of
  a comment.
- A multiple-character operator name cannot end in + or -, unless the name also contains at least
  one of these characters:
  ```
  ~!@#%^&|`?
  ```

### Special Characters

- A dollar sign (`$`) followed by digits is used to represent a positional parameter in the body of a
  function definition or a prepared statement.
  In other contexts the dollar sign can be part of an identifier or a dollar-quoted string constant.

- Parentheses (`()`) have their usual meaning to group expressions and enforce precedence.
  In some cases parentheses are required as part of the fixed syntax of a particular SQL command.

- Brackets (`[]`) are used to select the elements of an array.

- Commas (`,`) are used in some syntactical constructs to separate the elements of a list.
- The semicolon (`;`) terminates an SQL command.
  It cannot appear anywhere within a command, except within a string constant or quoted identifier.
- The colon (`:`) is used to select “slices” from arrays.
  In certain SQL dialects (such as Embedded SQL), the colon is used to prefix variable names.
- The asterisk (`*`) is used in some contexts to denote all the fields of a table row or composite value.
  It also has a special meaning when used as the argument of an aggregate function, namely that the
  aggregate does not require any explicit parameter.
- The period (`.`) is used in numeric constants, and to separate schema, table, and column names.

### Comments

```
-- This is a standard SQL comment
Alternatively, C-style block comments can be used:
/* multiline comment
* with nesting: /* nested block comment */
*/
```

### Operator Precedence

**Operator Precedence (highest to lowest)**

| Operator/Element              | Associativity | Description                                        |
| ----------------------------- | ------------- | -------------------------------------------------- |
| .                             | left          | table/column name separator                        |
| ::                            | left          | PostgreSQL-style typecast                          |
| []                            | left          | array element selection                            |
| +-                            | right         | unary plus, unary minus                            |
| ^                             | left          | exponentiation                                     |
| \*/%                          | left          | multiplication, division, modulo                   |
| +-                            | left          | addition, subtraction                              |
| (any other operator)          | left          | all other native and user-defined operators        |
| BETWEEN IN LIKE ILIKE SIMILAR |               | range containment, set membership, string matching |
| <> = <= >= <>                 |               | comparison operators                               |
| IS ISNULL NOTNULL             |               | IS TRUE, IS FALSE, IS NULL, IS DISTINCT FROM, etc. |
| NOT                           | right         | logical negation                                   |
| AND                           | left          | logical conjunction                                |
| OR                            | left          | logical disjunction                                |

The operator precedence rules also apply to user-defined operators that have the same names
as the built-in operators mentioned above.

The `OPERATOR` construct is taken to have the default precedence for "any other operator".

## 2 Value Expressions

The result of a value expression is sometimes called a **scalar**,
to distinguish it from the result of a table expression (which is a table).
Value expressions are therefore also called **scalar expressions** (or even simply **expressions**).

The expression syntax allows the calculation of values from primitive parts using
arithmetic, logical, set, and other operations.

A value expression is one of the following:

### A constant or literal value

---

### A column reference

```sql
correlation.columnname
```

The correlation name and separating dot can be omitted if the
column name is unique across all the tables being used in the current query.

---

### A positional parameter reference, in the body of a function definition or prepared statement

```sql
$number
```

A positional parameter reference is used to indicate a value that is supplied externally to an SQL statement.
Parameters are used in SQL function definitions and in prepared queries.

---

### A subscripted expression

If an expression yields a value of an array type:

```sql
expression[subscript] -- use this to extract a specific element of the array value
expression[lower_subscript:upper_subscript] -- use this to extract multiple adjacent elements
```

---

### A field selection expression

If an expression yields a value of a composite type (row type), then a specific field of the row can
be extracted by writing

```sql
expression.fieldname
```

> In general the row expression must be parenthesized,
> but the parentheses can be omitted when the expression
> to be selected from is just a table reference or positional parameter.

- An operator invocation

  ```sql
  expression operator expression -- binary infix operator
  operator expression -- unary prefix operator
  ```

  ```sql
  OPERATOR(schema.operatorname) -- a qualified operator name
  ```

---

### A function call

```sql
function_name ([expression [, expression ... ]] )
```

A function that takes a single argument of composite type can optionally be called using field-
selection syntax, and conversely field selection can be written in functional style.
`col(table)` and `table.col` are interchangeable.

---

### An aggregate expression

An **aggregate expression** represents the application of
an aggregate function across the rows selected by a query.
An aggregate function reduces multiple inputs to a single output value

```sql
aggregate_name (expression [ , ... ] [ order_by_clause ] ) [ FILTER -- once for each input row
( WHERE filter_clause ) ]

aggregate_name (ALL expression [ , ... ] [ order_by_clause ] )      -- once for each input row
[ FILTER ( WHERE filter_clause ) ]

aggregate_name (DISTINCT expression [ , ... ] [ order_by_clause ] ) -- once for each distinct value of the expression
[ FILTER ( WHERE filter_clause ) ]

aggregate_name ( * ) [ FILTER ( WHERE filter_clause ) ]             -- once for each input row

aggregate_name ( [ expression [ , ... ] ] ) WITHIN GROUP            -- used with ordered-set aggregate functions
( order_by_clause ) [ FILTER ( WHERE filter_clause ) ]
```

_expression_ is any value expression that does not itself contain
an aggregate expression or a window function call.

Most aggregate functions ignore null inputs, unless otherwise specified.
the optional `order_by_clause` can be used to specify the desired ordering
When dealing with multiple-argument aggregate functions, note that the `ORDER BY` clause goes after
all the aggregate arguments
If DISTINCT is specified in addition to an `order_by_clause`, then all the ORDER BY expressions
must match regular arguments of the aggregate.

**Ordered-set aggregates** is a subclass of aggregate functions for which an `order_by_clause` is required.
The argument expressions preceding `WITHIN GROUP`, if
any, are called **direct arguments** to distinguish them from
the aggregated arguments listed in the `order_by_clause`.
Direct arguments are evaluated _only once per aggregate call_, not once per input row.

```sql
SELECT percentile_cont(0.5) WITHIN GROUP (ORDER BY income) FROM households;
```

If `FILTER` is specified, then only the input rows for which the `filter_clause` evaluates to true
are fed to the aggregate function; other rows are discarded.

> An aggregate expression can only appear in the result list or HAVING clause of a SELECT command.

---

### A window function call

A **window function** call represents the application of an aggregate-like function over some portion of
the rows selected by a query. Each row remains separate in the query output.

```sql
function_name ([expression [, expression ... ]]) [ FILTER
( WHERE filter_clause ) ] OVER window_name
function_name ([expression [, expression ... ]]) [ FILTER
( WHERE filter_clause ) ] OVER ( window_definition )
function_name ( * ) [ FILTER ( WHERE filter_clause ) ]
OVER window_name
function_name ( * ) [ FILTER ( WHERE filter_clause ) ] OVER
( window_definition )
```

where `window_definition` has the syntax

```sql
[ existing_window_name ]
[ PARTITION BY expression [, ...] ]
[ ORDER BY expression [ ASC | DESC | USING operator ] [ NULLS
{ FIRST | LAST } ] [, ...] ]
[ frame_clause ]
```

The optional `frame_clause` can be one of

```sql
{ RANGE | ROWS | GROUPS } frame_start [ frame_exclusion ]
{ RANGE | ROWS | GROUPS } BETWEEN frame_start AND frame_end
[ frame_exclusion ]
```

where `frame_start` and `frame_end` can be one of

```sql
UNBOUNDED PRECEDING
offset PRECEDING
CURRENT ROW
offset FOLLOWING
UNBOUNDED FOLLOWING
```

and `frame_exclusion` can be one of

```sql
EXCLUDE CURRENT ROW
EXCLUDE GROUP
EXCLUDE TIES
EXCLUDE NO OTHERS
```

`expression` represents any value expression that does not itself contain window function calls.

The `PARTITION BY` clause groups the rows of the query into partitions,
which are processed separately by the window function.

The `frame_clause` specifies the set of rows constituting the window frame, which is a subset of
the current partition, for those window functions that act on the frame instead of the whole partition.
If `frame_end` is omitted, the end defaults to CURRENT ROW.

In the _offset_ `PRECEDING` and _offset_ `FOLLOWING` frame options, the offset must be an
expression not containing any variables, aggregate functions, or window functions. The meaning of
the _offset_ depends on the frame mode:

- In `ROWS` mode, the offset must yield a non-null, non-negative integer, and the option means that
  the frame starts or ends the specified number of rows before or after the current row.
- In `GROUPS` mode, the offset again must yield a non-null, non-negative integer, and the option
  means that the frame starts or ends the specified number of peer groups before or after the current
  row's peer group, where a peer group is a set of rows that are equivalent in the ORDER BY ordering.
  (There must be an ORDER BY clause in the window definition to use GROUPS mode.)
- In `RANGE` mode, these options require that the ORDER BY clause specify exactly one column. The
  offset specifies the maximum difference between the value of that column in the current row and
  its value in preceding or following rows of the frame. The data type of the offset expression varies
  depending on the data type of the ordering column. For numeric ordering columns it is typically
  of the same type as the ordering column, but for datetime ordering columns it is an interval.
  For example, if the ordering column is of type date or timestamp, one could write RANGE
  BETWEEN '1 day' PRECEDING AND '10 days' FOLLOWING. The offset is still
  required to be non-null and non-negative, though the meaning of “non-negative” depends on its
  data type.

The `frame_exclusion` option allows rows around the current row to be excluded from the frame,
even if they would be included according to the frame start and frame end options.

If `FILTER` is specified, then only the input rows for which the `filter_clause` evaluates to true
are fed to the window function

Window function calls are permitted only in the `SELECT` list and the `ORDER BY` clause of the query.

---

### A type cast

A **type cast** specifies a conversion from one data type to another.

```sql
CAST ( expression AS type ) -- conforms to SQL
expression::type            -- historical
```

When a cast is applied to a value expression of a known type, it represents a run-time type conversion.
The cast will succeed only if a suitable type conversion operation has been defined.

An explicit type cast can usually be omitted if there is no ambiguity as
to the type that a value expression must produce

---

### A collation expression

The `COLLATE` clause overrides the collation of an expression.
It is appended to the expression it applies to:

```sql
expr COLLATE collation
```

---

### A scalar subquery

A **scalar subquery** is an ordinary SELECT query in parentheses that returns exactly
one row with one column.

```sql
SELECT name, (SELECT max(pop) FROM cities WHERE cities.state = states.name)
    FROM states;
```

---

### An array constructor

An **array constructor** is an expression that builds an array value using values for its member elements.

A simple array constructor consists of the key word `ARRAY`, a left square bracket `[`, a list of expressions
(separated by commas) for the array element values, and finally a right square bracket `]`.

```sql
SELECT ARRAY[1,2,3+4];
```

Multidimensional array values can be built by nesting array constructors.
In the inner constructors, the key word `ARRAY` can be omitted.

---

### A row constructor

A **row constructor** is an expression that builds a row value (also called a composite value) using values
for its member fields.

A row constructor consists of the key word ROW, a left parenthesis, zero
or more expressions (separated by commas) for the row field values, and finally a right parenthesis.

```sql
SELECT ROW(1,2.5,'this is a test');
```

By default, the value created by a `ROW` expression is of an anonymous record type.
Row constructors can be used to build composite values to be stored in a composite-type table column,
or to be passed to a function that accepts a composite parameter.

---

### Another value expression in parentheses (used to group subexpressions and override precedence)

---

### Expression Evaluation Rules

The order of evaluation of subexpressions is not defined. In particular, the inputs of an operator or
function are not necessarily evaluated left-to-right or in any other fixed order.

> If the result of an expression can be determined by evaluating only some parts of it,
> then other subexpressions might not be evaluated at all.

To force evaluation order a `CASE` construct can be used.

Sometimes instead of `CASE` you have to use a `WHERE` or `FILTER` clause to prevent
problematic input rows from reaching an aggregate function in the first place.

```sql
SELECT ... WHERE CASE WHEN x > 0 THEN y/x > 1.5 ELSE false END;
```

## 3 Calling Functions

PostgreSQL allows functions that have named parameters to be called using either

- **positional notation**.

  ```sql
  SELECT foo('first', 'second', 3);
  ```

  Argument values are written in the same order as they
  are defined in the function declaration.

- **named notation**.

  ```sql
  SELECT foo(first => 'first', third => 3, second => 'second');
  ```

  Argument values are written in any order.

In either notation, parameters that have default values given in the function declaration need not be
written in the call at all.

PostgreSQL also supports **mixed notation**(positional parameters are written first),
which combines positional and named notation.

```sql
SELECT foo('first', third => 3, second => 'second');
```

> Named and mixed call notations currently cannot be used when calling an aggregate function
> (but they do work when an aggregate function is used as a window function).
