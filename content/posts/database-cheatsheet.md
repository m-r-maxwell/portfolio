+++ 
draft = false
date = 2024-05-19T16:23:59-04:00
title = "Database Cheatsheet"
description = "Some advanced DB options"
slug = ""
authors = []
tags = []
categories = ["database"]
externalLink = ""
series = ["database"]
+++

# Indexes
- Small data structure that can locate an item in a larger data structure
- Most databases use a form binary tree for indexes

## Use Cases
- Rapid look up
- Enforce constraints
- Ordered queries

## Costs
- Write Operations (such as having to write to a table and write a new index) and affect insert, update, drop
- Storage space

## When to use
- Primary key
- Columns used for ORDER BY
- Columns used in WHERE clauses

### Other information
UNIQUE constraint will create an index
Can manually define indexes in table definitions with
INDEX <name> (col_name) OR
UNIQUE INDEX <name> (col_name)

Creating an index afterwards can be done with:
CREATE INDEX name ON table(col_name)

Need to get rid of an index?
DROP INDEX name ON table

But what about multi-column indexes?
INDEX name (col_1, col2)

# Views

- Views are virtual tables that represent the results of a pre-defined query.
- They simplify complex queries and improve code reusability.

## Types of Views:
- Simple Views: Based on a single SELECT statement (as you already have in your example).
- Updatable Views: Under certain conditions, you can modify data through a view (Note: Updatability of views depends on the database system you're using).

Ex view:
```SQL
CREATE VIEW fireType AS
SELECT
    pokedex_number,
    name,
    type1,
    type2,
    stats
FROM pokemon.pokemon
WHERE type1 = 'fire' OR type2 = 'fire';

SELECT * FROM fireType; -- uses the new view
```

# Stored Functions and Procedures

## Functions vs Procedures
- Functions: Return a single value. They can be used in expressions and queries.
- Procedures: Do not return a value and are called separately. They are often used for performing side effects (e.g., modifying data).
- Both promote code reusability and modularity

## Additional notes
- Stored procedures can also return output parameters to modify variables outside the procedure.

Ex function:
```sql
CREATE FUNCTION testfunc(s VARCHAR(255))
RETURNS VARCHAR(255)
    BEGIN
        RETURN CONCAT('Hello, ' s, '!');
    END

SELECT testfunc('matt');
-- Hello, matt!
```

Ex procedure:
```sql
CREATE PROCEDURE testproc(n VARCHAR(16))
BEGIN
    SELECT CONCAT('Hello, ', n, '!')
END //
CALL testproc('matt')
-- Hello, matt!
```

### Removing Either
```sql
DROP FUNCTION IF EXISTS fnname
DROP PROCEDURE IF EXISTS procname
```

### Functions
- Cannot be used in an aggregate context but can be used on the results of an aggregate function

# Transaction
Transaction Isolation Levels: Transactions have different isolation levels that control how concurrent transactions interact with each other. These levels can impact performance and data consistency. (You can find more information about isolation levels in the database system's documentation).
Transactions are commonly used to increase performance letting each query inside process, especially in large inserts or updates.

Ex transaction:
```sql
DROP TABLE IF EXISTS widgetInventory;
DROP TABLE IF EXISTS widgetSales;
CREATE TABLE widgetInventory (
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    description TEXT,
    onhand INTEGER NO NULL
);

CREATE TABLE widgetSales(
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    inv_id INTEGER,
    quan INTEGER,
    price INTEGER
);
-- start tx
START TRANSACTION;
INSERT INTO widgetSales (inv_id, quan, price) VALUES (1, 5, 500);
UPDATE widgetInventory SET onhand = (onhand - 5) WHERE id = 1;
COMMIT; -- end tx
-- ROLLBACK; this will stop the transaction and reset the database
```

# Triggers
Triggers are operations that are automatically performed when a specifc database event occurs.
If you want to show triggers you can use `SHOW TRIGGERS;`

## Cautions:
- Triggers can add complexity to your database and can impact performance.
- Use triggers judiciously and consider alternative approaches (e.g., application logic) when appropriate.

To drop a trigger you can run:
```sql
DROP TRIGGER IF EXISTS triggerName;
```

Ex trigger:
```sql
DROP TRIGGER IF EXISTS newWidgetSale;
DELIMITER //
CREATE TRIGGER newWidgetSale AFTER INSERT ON widgetSales
    FOR EACH ROW
        BEGIN
            UPDATE widgetCustomers
            SET last_order_ID = NEW.id
            WHERE id = NEW.customer_id;
        END //
DELIMITER ;
```

Triggers can also be used to prevent updates.

Ex:

```sql
DELIMITER //
CREATE TRIGGER updateWidgetSales BEFORE UPDATE ON widgetSales
    FOR EACH ROW
    BEGIN
        IF OLD.id = NEW.id AND OLD.reconciled = 1 THEN
            SIGNAL SQLSTATE '45000' set message_text = 'cannot update reconciled row: "widgetSale"';
        END IF
    END //
DELIMITER ;
```


# Foreign Keys
Foreign keys refer to a column in another table. Foreign keys are used to maintain integrity between two tables.