
## Uppercase and lowercase ##
##### Use uppercase for statements, operators and keywords.
```sql
-- bad
SELECT * FROM foo order by bar;

-- good
SELECT * FROM foo ORDER BY bar;
```

##### Use lowercase for functions.
```sql
-- bad
SELECT COUNT(*) FROM foo;

-- good
SELECT count(*) FROM foo;
```

##### Use uppercase for types.
```sql
-- bad
CREATE TABLE foo(bar integer, baz text);

-- good
CREATE TABLE foo(bar INTEGER, baz TEXT);
```

## Operators ##
##### Use same operators as SQL Server.
```sql
-- good
SELECT baz FROM foo WHERE bar <> baz;
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
CREATE FUNCTION foo(in par_text = 'bar') RETURNS text AS $$
  SELECT par_text;
$$ LANGUAGE sql;

-- good
CREATE FUNCTION foo(in text DEFAULT 'bar') RETURNS text AS $$
  SELECT par_bar;
$$ LANGUAGE sql;
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
CREATE TABLE foo(a, b default 0, c, d default 1);

-- bad
INSERT INTO foo VALUES (1, 2, 3, 4);

-- bad
INSERT INTO foo(a, c) VALUES (1, 3);

-- good
INSERT INTO foo(a, b, c, d) VALUES (1, 2, 3, 4);

-- good
INSERT INTO foo(a, b, c, d) VALUES (1, default, 3, default);
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
CREATE TABLE foo ();
INSERT INTO bar (baz) VALUES (1);
INSERT INTO bar(baz) VALUES(1);
CREATE FUNCTION foo_bar (in_a text) ...;

-- good
CREATE TABLE foo();
INSERT INTO bar(baz) VALUES (1);
CREATE FUNCTION foo_bar(in_a text) ...;
```

##### Use space before parentheses and comma dangle at line end when using multi-line code.
```sql
-- good
CREATE TABLE foo
(
  a,
  b,
  c
);

-- bad
CREATE table foo (
  a
  , b
  , c
);
```

## Functions indentation ##
##### Respect proper function indentation, name the `$$` block and use multi-line parameters only if needed.
```sql
-- good
CREATE FUNCTION foo(in_a text, in_b integer)
RETURNS INTEGER
AS 
$body$
    SELECT 
        foo_id,
        ff
    FROM foo a
    INNER JOIN boo AS b
        ON a. = b.
    WHERE bar = in_a 
        AND baz = in_b
    ORDER BY bar;
$body$ 
LANGUAGE sql;
```
