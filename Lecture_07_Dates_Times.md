# 7. Dates and Times

## 7.1 Standard format
- `2018-03-28 15:32:47`
    - helps sorting and searching operations
- stored as UTC (coordinated universal time)
    - allows unambigious date time calculations across geographical locations 
- SQL date and time types
    - DATE, TIME, DATETIME, YEAR, INTERVAL

## 7.2 DATE/TIME functions
-- :memory:

- `SELECT DATETIME('now');`
    - returns current data and time in UTC
    - SQL uses a string to represent this
- `SELECT DATE('now');`
    - returns only date
- `SELECT TIME('now');`
    - returns only time
<br><br>
Datetime arithmetic

- `SELECT DATETIME('now', '+1 day');`
    - now + 1 day
- `SELECT DATETIME('now', '+3 days');`
- `SELECT DATETIME('now', '-1 month');`
    - 1 month ago
- `SELECT DATETIME('now', '+1 year');`
    - 1 year in the future
- `SELECT DATETIME('now', '+3 hours', '+27 minutes', '-1 day', '+3 years');`
