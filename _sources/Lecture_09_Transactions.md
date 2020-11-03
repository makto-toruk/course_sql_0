# 9. Transactions

## 9.1 definition
- transaction is a group of operations that are handled as one unit of work.
- if any of these fails, the group is treated as failed. Db is restored to its state before group of operations
    - this is useful when applying only some operations may not make sense.
- can also speed up performance by performing for example many write operations together (before making checks in memory)

## 9.2 transactions
```sql
-- test.db
CREATE TABLE widgetInventory (
  id INTEGER PRIMARY KEY,
  description TEXT,
  onhand INTEGER NOT NULL
);
--
CREATE TABLE widgetSales (
  id INTEGER PRIMARY KEY,
  inv_id INTEGER,
  quan INTEGER,
  price INTEGER
);
--
INSERT INTO widgetInventory ( description, onhand ) VALUES ( 'rock', 25 );
INSERT INTO widgetInventory ( description, onhand ) VALUES ( 'paper', 25 );
INSERT INTO widgetInventory ( description, onhand ) VALUES ( 'scissors', 25 );
```

- Suppose you have the above two tables. Suppose we sell something from inventory, both the Sales and Inventory table need to be updated. It doesn't make sense to update only one.
  ```sql
  BEGIN TRANSACTION;
  INSERT INTO widgetSales ( inv_id, quan, price ) VALUES ( 1, 5, 500 );
  UPDATE widgetInventory SET onhand = ( onhand - 5 ) WHERE id = 1;
  END TRANSACTION;
  ```
  - ensures both are performed.

## 9.3 Performance
Example:
```sql
BEGIN TRANSACTION;
-- copy / paste 1,000 of these ...
INSERT INTO test ( data ) VALUES ( 'this is a good sized line of text.' );
-- put this after the 1,000 INSERT statements
END TRANSACTION;
```
- Without the begin and end transaction, the speed in SQLite is ~20 times slower.