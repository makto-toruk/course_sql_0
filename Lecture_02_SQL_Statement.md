# 2. SQL Statement

## 2.1 SQL Overview
- SQL statements are terminated with `;`

Example:
- `SELECT * FROM Countries WHERE Continent = 'Europe';`
    - FROM clause: `FROM Countries`
    - WHERE clause: `WHERE Continent = 'Europe';` consists of logical expression.
These parts together constitute a statement.

- CRUD: 4 fundamental database system functions. Create Read Update Delete.
1. SELECT: all queries that return values. (R in CRUD)
2. INSERT: add a row to a table (C in CRUD).
    - example:
    ```sql
    INSERT INTO Customer (name, city, state)
        VALUES ('Jimi Hendrix', 'Renton', 'WA');
    ```
    - Broken into two lines to improve readability.
3. UPDATE: to change data (U in CRUD)
    - example: 
    ```sql
    UPDATE customer 
    SET 
        address = '123 Music Avenue', 
        zip = '98056' 
    WHERE id = 5;
    ```
4. DELETE: remove rows in a table (D in CRUD).
    - example: `DELETE FROM Customer WHERE id = 4;`
    
## 2.2 Database Organization
- SQL is a language for managing a relational database.
- Relational database: organized in two-dimensional tables (rows and columns).
- Database can contain multiple tables.
- Table contains rows and columns.
- A row is an individual _record_ in a table.
- A column is a _field_.
- Each row in a table has a unique key. Sometimes hidden, but table must have one. 
- Tables are related by keys.
    - example table: `id | item_id | cust_id | quant | price` where `item_id` and `cust_id` are keys of other tables.
    - helps use joint queries.

## 2.3 SELECT
examples:
```sql
-- world.db
```
- `SELECT 'Hello, World';`
    - SELECT is a key word. Doesn't have to be capitalized, but common practice.
- `SELECT 'Hello, World' AS Result;`
    - Loads 'Hello, World' under column Result
- `SELECT * FROM Country;`
    - `*` indicates select all columns
- `SELECT * FROM Country ORDER BY Name;`
    - `ORDER BY` clause
- `SELECT Name, LifeExpectancy FROM Country ORDER BY Name;`
    - selected columns
- `SELECT Name, LifeExpectancy AS "Life Expectancy" FROM Country ORDER BY Name;`
    - if LifeExpectancy isn't readable, load AS. delimit using double quotes (since there is a space). Single quotes are typically used for literal string.
    - some systems allow ignoring the keyword `AS`
        - `SELECT Name, LifeExpectancy "Life Expectancy" FROM Country ORDER BY Name;`

## 2.4 WHERE
Selecting Rows
```sql
-- world.db
```
- `SELECT Name, Continent, Region FROM Country;`
    - selects every row with the specified columns.
- `SELECT Name, Continent, Region FROM Country WHERE Continent = 'Europe';`
    - limits to rows `WHERE` continent is Europe. Europe is a literal string, so use single quotes. Without single quotes, it looks for column with name Europe.
- `SELECT Name, Continent, Region FROM Country WHERE Continent = 'Europe' ORDER BY Name;`
    - can ORDER as usual.
- `SELECT Name, Continent, Region FROM Country WHERE Continent = 'Europe' ORDER BY Name LIMIT 5;`
    - limit to 5 rows.
- `SELECT Name, Continent, Region FROM Country WHERE Continent = 'Europe' ORDER BY Name LIMIT 5 OFFSET 5;`
    - starts the limit after `OFFSET` (or after the first 5).    
- `ORDER BY` has to be _after_ any `WHERE` clause
- `LIMIT` has to be last
- `FROM` needs to be first

## 2.5 Columns
Selecting Columns
```sql
-- world.db
SELECT * FROM Country;
SELECT Name, Continent, Region FROM Country;
SELECT Name AS Country, Continent, Region FROM Country;
SELECT Name AS Country, Region, Continent FROM Country;
```
We've seen all these before.

## 2.6 COUNT
Counting Rows
```sql
-- world.db
```
- `SELECT COUNT(*) FROM Country;`
    - counts all the rows
- `SELECT COUNT(*) FROM Country WHERE Population > 1000000;`
    - counts all the rows where population > 1000000
- `SELECT COUNT(*) FROM Country WHERE Population > 100000000;`
- `SELECT COUNT(*) FROM Country WHERE Population > 100000000 AND Continent = 'Europe' ;`
    - can add conditions other than population
- `SELECT COUNT(*) FROM Country;`
    - this counts ALL rows
- `SELECT COUNT(LifeExpectancy) FROM Country;`
    - only counts rows where "LifeExpectancy" has some data.
    
## 2.7 INSERT
Inserting Data
```sql
-- test.db
```
- `SELECT * FROM customer;`

- To add a new row:
    ```sql
    INSERT INTO customer (name, address, city, state, zip) 
        VALUES ('Fred Flintstone', '123 Cobblestone Way', 'Bedrock', 'CA', '91234');
    ```
- To populate some columns in a row:
    ```sql
    INSERT INTO customer (name, city, state) 
        VALUES ('Jimi Hendrix', 'Renton', 'WA');
    ```
- `SELECT COUNT(address) FROM customer` returns 4
- Note: can use `id` as a field in insert
- but if `id` exists, then SQL returns error. i.e, insert can't overwrite

## 2.8 UPDATE
Updating Data (or Changing data)
```sql
-- test.db
SELECT * FROM customer;
UPDATE customer SET address = '123 Music Avenue', zip = '98056' WHERE id = 5;
-- or `UPDATE customer SET address = '123 Music Avenue', zip = '98056' WHERE name = 'Jimi Hendrix';
UPDATE customer SET address = '2603 S Washington St', zip = '98056' WHERE id = 5;
UPDATE customer SET address = NULL, zip = NULL WHERE id = 5;
```
- `NULL` is a special value (no data)
    
## 2.9 DELETE
Deleting Data
```sql
-- test.db
SELECT * FROM customer WHERE id = 4;
DELETE FROM customer WHERE id = 4;
SELECT * FROM customer;
DELETE FROM customer WHERE id = 5;
SELECT * FROM customer;
```
- Note: if you want to delete an entry in a column, `UPDATE` to `NULL`
- If you want to delete an entire row, then use `DELETE`