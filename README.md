Perfect. Here's how to wrap the refresh logic in a **stored procedure** so you can manually trigger the refresh of your materialized view anytime you want, in addition to the scheduled daily event.

---

## ✅ Final Setup: Stored Procedure + Event Scheduler + Logging

### 1️⃣ Create the Materialized View Table (if not already created)

```sql
CREATE TABLE IF NOT EXISTS employee_summary_mv (
  department_id INT,
  total_employees INT
);
```

---

### 2️⃣ Create the Logging Table

```sql
CREATE TABLE IF NOT EXISTS mv_refresh_log (
  id INT AUTO_INCREMENT PRIMARY KEY,
  refresh_time DATETIME,
  status VARCHAR(50),
  message TEXT
);
```

---

### 3️⃣ Create the Stored Procedure

```sql
DELIMITER $$

CREATE PROCEDURE sp_refresh_employee_summary_mv()
BEGIN
  DECLARE exit HANDLER FOR SQLEXCEPTION
  BEGIN
    -- Log the failure
    INSERT INTO mv_refresh_log (refresh_time, status, message)
    VALUES (NOW(), 'FAILED', 'Error during refresh procedure');
  END;

  -- Create temporary staging table
  CREATE TEMPORARY TABLE IF NOT EXISTS employee_summary_mv_temp (
    department_id INT,
    total_employees INT
  );

  -- Clear previous temp data (in case it exists)
  DELETE FROM employee_summary_mv_temp;

  -- Insert fresh data into temp table
  INSERT INTO employee_summary_mv_temp
  SELECT department_id, COUNT(*) AS total_employees
  FROM employees
  GROUP BY department_id;

  -- Replace contents of materialized view table
  DELETE FROM employee_summary_mv;
  INSERT INTO employee_summary_mv
  SELECT * FROM employee_summary_mv_temp;

  -- Log the success
  INSERT INTO mv_refresh_log (refresh_time, status, message)
  VALUES (NOW(), 'SUCCESS', 'Refreshed successfully');
END$$

DELIMITER ;
```

---

### ✅ 4️⃣ Create the Scheduled Event to Call the Stored Procedure

```sql
DELIMITER $$

CREATE EVENT IF NOT EXISTS refresh_employee_summary_mv_event
ON SCHEDULE EVERY 1 DAY
STARTS TIMESTAMP(CURRENT_DATE + INTERVAL 1 DAY) + INTERVAL 2 HOUR
DO
BEGIN
  CALL sp_refresh_employee_summary_mv();
END$$

DELIMITER ;
```

---

### 🚀 5️⃣ Manually Run the Refresh Anytime

You can now trigger the refresh manually like this:

```sql
CALL sp_refresh_employee_summary_mv();
```

---

### 🔍 6️⃣ Monitor Refresh Logs

View the latest refresh results:

```sql
SELECT * FROM mv_refresh_log ORDER BY refresh_time DESC LIMIT 10;
```

---

## ✅ You're All Set

With this setup:

* The materialized view (`employee_summary_mv`) is refreshed daily at 2:00 AM.
* You can refresh it manually using the stored procedure.
* All refresh attempts (success or failure) are logged.
* Data is safely staged using a temporary table to avoid incomplete updates.

Great question! Multiple queries in a **stored procedure** are often necessary for **real-time, multi-step logic** — just like how code in a regular application may need to:

1. Check a condition,
2. Update data based on that condition,
3. Log the result,
4. Handle errors or rollback if needed.

---

## 🧠 **Why Multiple Queries Are Needed in Stored Procedures**

Stored procedures let you group **multiple SQL statements** together to perform a task as one unit. This is essential when:

* The task requires **conditional logic**.
* You need to **ensure consistency** across steps (often using transactions).
* You want to **avoid network round-trips** for performance.
* You want to **log** or **handle errors** gracefully.

---

## 📦 Real-Time Use Case Example: **Order Processing System**

### 🛒 Scenario: Placing an Order in an E-commerce System

Let’s say a user places an order. The system needs to:

1. **Check if the product is in stock**.
2. **Deduct inventory**.
3. **Create the order record**.
4. **Insert a billing record**.
5. **Log the transaction**.
6. **Rollback** everything if anything fails.

You **can’t** do that in a single SQL query — you need a **stored procedure**.

---

### ✅ Example: Order Processing Procedure

```sql
DELIMITER $$

CREATE PROCEDURE place_order(
  IN product_id INT,
  IN customer_id INT,
  IN quantity INT
)
BEGIN
  DECLARE stock_available INT DEFAULT 0;

  DECLARE EXIT HANDLER FOR SQLEXCEPTION
  BEGIN
    ROLLBACK;
    INSERT INTO order_log (order_time, status, message)
    VALUES (NOW(), 'FAILED', 'Transaction rolled back due to error');
  END;

  START TRANSACTION;

  -- 1. Check stock
  SELECT stock INTO stock_available
  FROM products
  WHERE id = product_id FOR UPDATE;

  IF stock_available < quantity THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Insufficient stock';
  END IF;

  -- 2. Deduct stock
  UPDATE products
  SET stock = stock - quantity
  WHERE id = product_id;

  -- 3. Create order
  INSERT INTO orders (product_id, customer_id, quantity, order_date)
  VALUES (product_id, customer_id, quantity, NOW());

  -- 4. Create billing entry
  INSERT INTO billing (customer_id, amount, billing_date)
  VALUES (customer_id, quantity * 100, NOW()); -- assume fixed price

  -- 5. Log success
  INSERT INTO order_log (order_time, status, message)
  VALUES (NOW(), 'SUCCESS', 'Order placed successfully');

  COMMIT;
END$$

DELIMITER ;
```

---

### 🔍 Why Multiple Queries Here?

| Step                    | Why It’s Needed                        |
| ----------------------- | -------------------------------------- |
| `SELECT stock`          | To check if the item can be sold.      |
| `UPDATE products`       | To reserve the stock.                  |
| `INSERT INTO orders`    | To record the actual order.            |
| `INSERT INTO billing`   | To initiate payment or tracking.       |
| `INSERT INTO order_log` | For monitoring and troubleshooting.    |
| `ROLLBACK`              | To undo all actions if anything fails. |

---

## 🔄 Summary

**Multiple queries are essential in stored procedures** to execute a full business logic unit that depends on:

* Multiple data operations,
* Error handling,
* Conditional logic,
* Logging or auditing.

Let me know if you want a simpler or different domain example — like finance, HR, or IoT.
