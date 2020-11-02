# 5. Strings

## 5.1 Literal string
- literal string: series of characters enclosed in single quotes.
    - some use double quotes, but this is not standard.
- to use a single quote in a string, use two single quotes `''`
- Concatenation
    - standard SQL: `SELECT 'THIS' || ' & ' || 'that';`
    - other SQLs may have slightly different operators.
- String functions:
    - SUBSTR, LENGTH, TRIM, UPPER, LOWER
    - these vary from system-to-system

## 5.2 LENGTH
-- world.db

- `SELECT LENGTH('string');`
    - returns 6
- `SELECT Name, LENGTH(Name) AS Len FROM City ORDER BY Len DESC;`
    - can be ordered by length

## 5.3 SUBSTR
-- album.db

- `SELECT SUBSTR('this string', 6);`
    - second argument is starting position of substring (starting from 1)
- `SELECT SUBSTR('this string', 6, 3);`
    - third argument is number of characters to return

```sql
SELECT released,
    SUBSTR(released, 1, 4) AS year,
    SUBSTR(released, 6, 2) AS month,
    SUBSTR(released, 9, 2) AS day
  FROM album
  ORDER BY released
;
```
- notice that you can order by released (or date)
- SQL standard doesn't include a SUBSTR but most major systems have this feature (name and usage may differ)
    
## 5.4 TRIM

- `SELECT TRIM('   string   ');`
    - removes spaces
- `SELECT LTRIM('   string   ');`
    - removes spaces from the beginning of the string
- `SELECT RTRIM('   string   ');`
    - from the end of the string
- `SELECT TRIM('...string...', '.');`
    - second argument specifies what character to remove (space is default)
    
## 5.5 UPPER/LOWER
-- world.db
Used for normalizing data which makes comparing strings more effective.

- `SELECT 'StRiNg' = 'string';`
    - returns 0 which means False. Comparisons are case-sensitive
- `SELECT LOWER('StRiNg') = LOWER('string');`
    - returns 1
- `SELECT UPPER('StRiNg') = UPPER('string');`
    - also returns 1
- `SELECT UPPER(Name) FROM City ORDER BY Name;`
    - only converts ASCII characters (special/accented characters are not converted) 
- `SELECT LOWER(Name) FROM City ORDER BY Name;`
