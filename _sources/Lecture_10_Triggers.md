# 10. Triggers

## 10.1 update triggers
-- test.db

Example tables:
```sql
CREATE TABLE widgetCustomer ( id INTEGER PRIMARY KEY, name TEXT, last_order_id INT );
CREATE TABLE widgetSale ( id INTEGER PRIMARY KEY, item_id INT, customer_id INT, quan INT, price INT );`

```sql
INSERT INTO widgetCustomer (name) VALUES ('Bob');
INSERT INTO widgetCustomer (name) VALUES ('Sally');
INSERT INTO widgetCustomer (name) VALUES ('Fred');
```
<br><br>
If you want to manipulate a table whenever another table is updated, use a trigger.
```sql
CREATE TRIGGER newWidgetSale AFTER INSERT ON widgetSale
BEGIN
UPDATE widgetCustomer SET last_order_id = NEW.id WHERE widgetCustomer.id = NEW.customer_id;
END
;
```
- NEW keyword refers to a virtual row

```sql
INSERT INTO widgetSale (item_id, customer_id, quan, price) VALUES (1, 3, 5, 1995);
INSERT INTO widgetSale (item_id, customer_id, quan, price) VALUES (2, 2, 3, 1495);
INSERT INTO widgetSale (item_id, customer_id, quan, price) VALUES (3, 1, 1, 2995);
SELECT * FROM widgetSale;
SELECT * FROM widgetCustomer;
```

## 10.2 preventing updates
-- test.db

```sql
CREATE TABLE widgetSale ( id integer primary key, item_id INT, customer_id INTEGER, quan INT, price INT,
    reconciled INT );
INSERT INTO widgetSale (item_id, customer_id, quan, price, reconciled) VALUES (1, 3, 5, 1995, 0);
INSERT INTO widgetSale (item_id, customer_id, quan, price, reconciled) VALUES (2, 2, 3, 1495, 1);
INSERT INTO widgetSale (item_id, customer_id, quan, price, reconciled) VALUES (3, 1, 1, 2995, 0);
SELECT * FROM widgetSale;
```

Here a trigger is defined to prevent an update.
```sql
CREATE TRIGGER updateWidgetSale BEFORE UPDATE ON widgetSale
BEGIN
SELECT RAISE(ROLLBACK, 'cannot update table "widgetSale"') FROM widgetSale
WHERE id = NEW.id AND reconciled = 1;
END
;
```

- the following then raises an error: 'cannot update table "widgetSale"'
```sql
BEGIN TRANSACTION;
UPDATE widgetSale SET quan = 9 WHERE id = 2;
END TRANSACTION;
```

## 10.3 timestamps
Suppose you have the following tables:
```sql
CREATE TABLE widgetCustomer ( id integer primary key, name TEXT, last_order_id INT, stamp TEXT );
CREATE TABLE widgetSale ( id integer primary key, item_id INT, customer_id INTEGER, quan INT, price INT, stamp TEXT );
CREATE TABLE widgetLog ( id integer primary key, stamp TEXT, event TEXT, username TEXT, tablename TEXT, table_id INT);
INSERT INTO widgetCustomer (name) VALUES ('Bob');
INSERT INTO widgetCustomer (name) VALUES ('Sally');
INSERT INTO widgetCustomer (name) VALUES ('Fred');
```

- You can set a datetime stamp on when an update was made.

```sql
CREATE TRIGGER stampSale AFTER INSERT ON widgetSale
BEGIN
UPDATE widgetSale SET stamp = DATETIME('now') WHERE id = NEW.id;
UPDATE widgetCustomer SET last_order_id = NEW.id, stamp = DATETIME('now')
WHERE widgetCustomer.id = NEW.customer_id;
INSERT INTO widgetLog (stamp, event, username, tablename, table_id)
VALUES (DATETIME('now'), 'INSERT', 'TRIGGER', 'widgetSale', NEW.id);
END
;
```
- notice that several updates are made using this trigger
    
```sql
INSERT INTO widgetSale (item_id, customer_id, quan, price) VALUES (1, 3, 5, 1995);
INSERT INTO widgetSale (item_id, customer_id, quan, price) VALUES (2, 2, 3, 1495);
INSERT INTO widgetSale (item_id, customer_id, quan, price) VALUES (3, 1, 1, 2995);
```
- this updates all the tables with timestamps.