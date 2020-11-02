# 3. Fundamental Concepts

## 3.1 CREATE TABLE
```sql
-- test.db
CREATE TABLE test (
  a INTEGER,
  b TEXT
);
```
    - creates a table according to the provided parameters.
    - column definitions are called the 'database schema' or 'table schema' (part between the paranthesis)
    - note that last item can not have a comma (syntax error)
    

- `INSERT INTO test VALUES ( 1, 'a' );`
    - notice that column names can be skipped when adding all columns
- `INSERT INTO test VALUES ( 2, 'b' );`
- `INSERT INTO test VALUES ( 3, 'c' );`
- `SELECT * FROM test;`

## 3.2 DROP TABLE
```sql
-- test.db
```
- `CREATE TABLE test ( a TEXT, b TEXT );`
    - error: table already exists
    - if you want to create it differently, you must delete the  table first
- `INSERT INTO test VALUES ( 'one', 'two' );`
- `SELECT * FROM test;`
---
- `DROP TABLE test;`
    - use this to delete the entire table first
    - can use only if table exists
- `DROP TABLE IF EXISTS test;`
    - delete if it exists
    
## 3.3 INSERT INTO
```sql
-- test.db
```
- You can add one or many rows.
- `CREATE TABLE test ( a INTEGER, b TEXT, c TEXT );`
- `INSERT INTO test VALUES ( 1, 'This', 'Right here!' ); `
- `INSERT INTO test ( b, c ) VALUES ( 'That', 'Over there!' ); `
    - don't always have to add data to all columns
    - remaining rows are NULL
- `INSERT INTO test DEFAULT VALUES;`
    - adds a row of NULLs
- `INSERT INTO test ( a, b, c ) SELECT id, name, description from item;`
    - adds rows from a different table (all rows)

## 3.4 DELETE FROM
```sql
-- test.db
```
- `DELETE FROM test WHERE a = 3;`
    - deletes row where a = 3
- `SELECT * FROM test WHERE a = 1;`
    - good practice to view these columns before deleting
- `DELETE FROM test WHERE a = 1;`
    - deletes rows where a = 1
    
## 3.5 NULL
```sql
-- test.db
```
- `SELECT * FROM test WHERE a = NULL;`
    - does not work because NULL is not a value
- `SELECT * FROM test WHERE a IS NULL;`
    - correct way to query NULL value
- `SELECT * FROM test WHERE a IS NOT NULL;`
    - query non-NULL values
- `INSERT INTO test ( a, b, c ) VALUES ( 0, NULL, '' );`
    - 0 is not a NULL value
    - '' is not a NULL value
- `SELECT * FROM test WHERE b IS NULL;`
- `SELECT * FROM test WHERE b = '';`
    - doesn't return anything (NULL is not ' ')
- `SELECT * FROM test WHERE c = '';`
    - this works
- `SELECT * FROM test WHERE c IS NULL;`
    - '' is not NULL
---
- `DROP TABLE IF EXISTS test;`
- create new table with a NOT NULL constraint
```sql
CREATE TABLE test (
    a INTEGER NOT NULL,
    b TEXT NOT NULL,
    c TEXT
);
```    
- `INSERT INTO test VALUES ( 1, 'this', 'that' );`
- `SELECT * FROM test;`
<br><br>
- `INSERT INTO test ( b, c ) VALUES ( 'one', 'two' );`
- `INSERT INTO test ( a, c ) VALUES ( 1, 'two' );`
    - The above two fail
- `INSERT INTO test ( a, b ) VALUES ( 1, 'two' );`
    - This works
- `DROP TABLE IF EXISTS test;`

## 3.6 Constraints
```sql
-- test.db
```
- `CREATE TABLE test ( a TEXT, b TEXT, c TEXT );`
- `INSERT INTO test ( a, b ) VALUES ( 'one', 'two' );`
- `SELECT * FROM test;`
    - adds NULL to column `c` by default.
<br><br>
Replace first statement with
- `CREATE TABLE test ( a TEXT, b TEXT, c TEXT NOT NULL );`
    - Fails. Can't insert into two columns when the other column has a not null constraint
- `CREATE TABLE test ( a TEXT, b TEXT, c TEXT DEFAULT 'panda' );`
    - This works. Column `c` has panda has an entry
- `CREATE TABLE test ( a TEXT UNIQUE, b TEXT, c TEXT DEFAULT 'panda' );`
    - The unique constraint will not allow the same entry in multiple rows for that column. NULL is exempt from the unique constraint (most often, since NULL is not any particular value). Possible use in customer database.
- `CREATE TABLE test ( a TEXT UNIQUE NOT NULL, b TEXT, c TEXT DEFAULT 'panda' );`
    - to fix that, you can combine UNIQUE NOT NULL constraint


## 3.7 ALTER TABLE
- Sometimes you need to change the database schema after it has been defined.
```sql
-- test.db
CREATE TABLE test ( a TEXT, b TEXT, c TEXT );
INSERT INTO test VALUES ( 'one', 'two', 'three');
INSERT INTO test VALUES ( 'two', 'three', 'four');
INSERT INTO test VALUES ( 'three', 'four', 'five');
SELECT * FROM test;
```
---
- Suppose you want to add a column:
    - `ALTER TABLE test ADD d TEXT;`
- You can also use constraints:
    - `ALTER TABLE test ADD e TEXT DEFAULT 'panda';`
- This command should be carefully used. It may alter several other pieces of code.

## 3.8 ID
- Typically ID columns are automatically populated. This column hold a unique value for each row in a table.
- How you create it depends on the database system. These instructions are sepcific to SQLite.
```sql
-- test.db
CREATE TABLE test (
  id INTEGER PRIMARY KEY,
  a INTEGER,
  b TEXT
);
INSERT INTO test (a, b) VALUES ( 10, 'a' );
INSERT INTO test (a, b) VALUES ( 11, 'b' );
INSERT INTO test (a, b) VALUES ( 12, 'c' );
SELECT * FROM test;
DROP TABLE IF EXISTS test;
```

## 3.9 WHERE, LIKE, and IN
```sql
-- world.db
```
- How do we query the database for specific data? Using WHERE.
- `SELECT * FROM Country;`
    - Selects everything
- `SELECT Name, Continent, Population FROM Country 
  WHERE Population < 100000 ORDER BY Population DESC;`
    - Selects particular columns
        - where population < 100000
        - and orders by descending order of population
- `SELECT Name, Continent, Population FROM Country 
  WHERE Population < 100000 OR Population IS NULL ORDER BY Population DESC;`
    - remember NULL is not a value, so to include this add another clause to WHERE using the OR operator.
- `SELECT Name, Continent, Population FROM Country 
  WHERE Population < 100000 AND Continent = 'Oceania' ORDER BY Population DESC;`
    - we can also add another clause using the AND operator.
- `SELECT Name, Continent, Population FROM Country 
  WHERE Name LIKE '\%island\%' ORDER BY Name;`
    - the LIKE operator filters results 
    - the percent sign is a 'wildcard'. By placing on either sign, we query data which have island anywhere in the string.
    - if `'\%island'`, this queries data that end with island
    - to begin with island, `'island\%'`
    - another example `'_a%'` => first character can be anything, second character must be a string.
- `SELECT Name, Continent, Population FROM Country 
 WHERE Continent IN ( 'Europe', 'Asia' ) ORDER BY Name;`
     - to match values _in_ a list, the IN operator is used.
     
## 3.10 SELECT DISTINCT
```sql
-- world.db
```
- `SELECT Continent FROM Country;`
- `SELECT DISTINCT Continent FROM Country;`
    - Using the SELECT DISTINCT statement, you will get only unique results. 
---
```sql
-- test.db
DROP TABLE IF EXISTS test;
CREATE TABLE test ( a int, b int );
INSERT INTO test VALUES ( 1, 1 );
INSERT INTO test VALUES ( 2, 1 );
INSERT INTO test VALUES ( 3, 1 );
INSERT INTO test VALUES ( 4, 1 );
INSERT INTO test VALUES ( 5, 1 );
INSERT INTO test VALUES ( 1, 2 );
INSERT INTO test VALUES ( 1, 2 );
INSERT INTO test VALUES ( 1, 2 );
INSERT INTO test VALUES ( 1, 2 );
INSERT INTO test VALUES ( 1, 2 );
SELECT * FROM test;
```
- `SELECT DISTINCT a FROM test;`
    - shows only rows with distinct values in column `a`
- `SELECT DISTINCT b FROM test;`
    - shows only rows with distinct values in column `b`
- `SELECT DISTINCT a, b FROM test;`
    - shows only rows with distinct values in the pair (`a`, `b`).
    
## 3.11 ORDER BY
```sql
-- world.db
```
- `SELECT Name FROM Country;`
- `SELECT Name FROM Country ORDER BY Name;`
    - _sorts_ by the name column (ASCII order, default is ascending)
- `SELECT Name FROM Country ORDER BY Name DESC;`
    - _sorts_ be descending order
- `SELECT Name FROM Country ORDER BY Name ASC;`
    - same as default
- `SELECT Name, Continent FROM Country ORDER BY Continent, Name;`
    - you can also sort by multiple things. First by continent, and within continent, individual countries are also sorted
- `SELECT Name, Continent, Region FROM Country ORDER BY Continent DESC, Region, Name;`
    - you can make it more complicated with each being ASC or DESC

## 3.12 CASE
Use when you need conditional expressions
```sql
-- test.db
DROP TABLE IF EXISTS booltest;
CREATE TABLE booltest (a INTEGER, b INTEGER);
INSERT INTO booltest VALUES (1, 0);
SELECT * FROM booltest;
```
- 0 is considered false, anything else is considered true.

```sql
SELECT
    CASE WHEN a THEN 'true' ELSE 'false' END as boolA,
    CASE WHEN b THEN 'true' ELSE 'false' END as boolB
    FROM booltest
;
```
---
```sql
SELECT
    CASE a WHEN 1 THEN 'true' ELSE 'false' END AS boolA,
    CASE b WHEN 1 THEN 'true' ELSE 'false' END AS boolB 
    FROM booltest
;
DROP TABLE IF EXISTS booltest;
```