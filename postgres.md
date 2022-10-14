
## Uppercase and lowercase ##
##### Use uppercase for statements, operators and keywords.
```sql
-- bad
SELECT bar FROM foo order by bar;

-- good
SELECT bar FROM foo ORDER BY bar;
```

##### Use lowercase for function calls.
```sql
-- bad
SELECT COUNT(*) FROM foo;

-- good
SELECT count(*) FROM foo;
```

##### Use uppercase for types.
```sql
-- bad
CREATE TABLE IF NOT EXISTS foo(bar integer, baz text);

-- good
CREATE TABLE IF NOT EXISTS foo(bar INTEGER, baz TEXT);
```

## Operators ##
##### Use consistent operators.
```sql
-- good
EXECUTE 'SELECT baz FROM dbo.foo WHERE' || var_bar '<> baz';
```

## Naming ##
##### Use t$ prefix when naming temporary tables.
```sql
-- good
CREATE TEMPORARY TABLE IF NOT EXISTS t$foo(
    bar INTEGER,
    baz TEXT) ON COMMIT DROP;
```

## Arrays ##
##### Use proper array declaration.
```sql
-- bad
SELECT '{1, 2, 3 + 4}';

-- bad
SELECT array[1,2,3+4];

-- good
SELECT ARRAY[1, 2, 3 + 4];
```

##### Parameters with a default value must be declared with the `DEFAULT` keyword.
```sql
-- bad
CREATE OR REPLACE FUNCTION foo(in par_bar = 'bar') 
RETURNS TEXT 
AS 
$$
  SELECT bar;
$$ 
LANGUAGE sql;

-- good
CREATE OR REPLACE FUNCTION foo(in par_bar TEXT DEFAULT 'bar') 
RETURNS TEXT 
AS 
$$
  SELECT par_bar;
$$ 
LANGUAGE sql;
```

## Statements ##
##### Always specify the needed columns.
```sql
-- bad
SELECT * FROM foo;

-- good
SELECT foo_id, bar_id, creation_date FROM foo;
```

##### Specify table columns when inserting even if it is set to default.
```sql
CREATE TABLE IF NOT EXISTS foo(a, b DEFAULT 0, c, d DEFAULT 1);

-- bad
INSERT INTO foo VALUES (1, 2, 3, 4);

-- bad
INSERT INTO foo(a, c) VALUES (1, 3);

-- good
INSERT INTO foo(a, b, c, d) VALUES (1, 2, 3, 4);

-- good
INSERT INTO foo(a, b, c, d) VALUES (1, DEFAULT, 3, DEFAULT);
```

##### Separate columns and their aliases
```sql
-- bad
SELECT f.bar FROM foo f;

-- good
SELECT f.bar FROM foo AS f;
```

## Spaces and parentheses ##
##### Do not use space before parentheses when writing one-line code unless it is following a keyword.
```sql
-- bad
CREATE TABLE IF NOT EXISTS foo ();
INSERT INTO bar (baz) VALUES (1);
INSERT INTO bar(baz) VALUES(1);
CREATE OR REPLACE FUNCTION foo_bar (in par_a TEXT) ...;

-- good
CREATE TABLE IF NOT EXISTS foo();
INSERT INTO bar(baz) VALUES (1);
CREATE OR REPLACE FUNCTION foo_bar(in par_a TEXT) ...;
```

##### Use space before parentheses and comma dangle at line end when using multi-line code.
```sql
-- good
CREATE TABLE IF NOT EXISTS foo
(
  a,
  b,
  c
);

-- bad
CREATE TABLE IF NOT EXISTS foo (
  a
  , b
  , c
);
```

## Functions indentation ##
##### Respect proper function indentation, name the `$$` block and use multi-line parameters only if needed.
```sql
-- good
CREATE OR REPLACE FUNCTION foo(par_a TEXT, par_b INTEGER)
RETURNS TABLE (id INTEGER, par_b INTEGER)
AS 
$body$
    SELECT 
        a.id,
        par_b
    FROM dbo.foo AS a
    INNER JOIN boo AS b
        ON (
            par_b > 0
            AND par_a <> (par_b - 1)::TEXT
        )
    WHERE bar = in_a 
        AND baz = in_b
    ORDER BY bar;
$body$ 
LANGUAGE sql;
```
