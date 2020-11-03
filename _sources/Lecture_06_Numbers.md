# 6. Numbers

## 6.1 Numeric Types
- Datatypes differ from system-to-system. This chapter covers how it works in SQLite
- Fundamental numeric types: Integers and Real numbers
    - Integer: `INTEGER`, `DECIMAL`, `MONEY`
    - Real: `REAL`, `FLOAT` (sacrifice accuracy for scale)
- Precision vs Scale
    - precision: how many digits are represented
    - scale: magnitude of numbers are represented
        - floating point: large magnitude, few significant digits
        
## 6.2 typeof
- `SELECT TYPEOF( 1 + 1 );`
    - returns integer
- `SELECT TYPEOF( 1 + 1.0 );`
    - real
- `SELECT TYPEOF('panda');`
    - text
- `SELECT TYPEOF('panda' + 'koala');`
    - returns integer ?!?!?
    
## 6.3 INTEGER division
- `SELECT 1 / 2;`
    - returns 0 since 1 is an integer
- `SELECT 1.0 / 2;`
    - returns 0.5 since 1.0 is real
- `SELECT CAST(1 AS REAL) / 2;`
    - can get same result using one of the operands as a real number
- `SELECT 17 / 5;`
    - returns 3
- `SELECT 17 / 5, 17 % 5;`
    - modulo operator works as usual
    - some system may use a different operator for modulo
    
## 6.4 ROUND()
To round a numeric value to a specific number of decimal places
- `SELECT 2.55555;`
    - a real number with 5 places of precision
- `SELECT ROUND(2.55555);`
    - returns 3
- `SELECT ROUND(2.55555, 3);`
    - second argument rounds to that many decimal places
    - returns 2.556
- `SELECT ROUND(2.55555, 0);`
    - default, returns 3