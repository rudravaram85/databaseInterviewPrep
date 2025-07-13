-- Schema for Banking Domain
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL
);

CREATE TABLE accounts (
    account_number INT PRIMARY KEY,
    customer_id INT,
    balance DECIMAL(12, 2),
    branch_id INT,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

-- Schema for E-commerce Domain
CREATE TABLE ecommerce_customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL
);

CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10, 2),
    discontinued BOOLEAN DEFAULT FALSE
);

CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    product_id INT,
    quantity INT,
    status VARCHAR(20),
    FOREIGN KEY (customer_id) REFERENCES ecommerce_customers(customer_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);

-- Seed Data for Banking Domain
INSERT INTO customers VALUES (1, 'John Doe');
INSERT INTO customers VALUES (2, 'Jane Smith');
INSERT INTO customers VALUES (3, 'Michael Johnson');
INSERT INTO customers VALUES (4, 'Emily Davis');
INSERT INTO customers VALUES (5, 'William Brown');
INSERT INTO customers VALUES (6, 'Linda Wilson');
INSERT INTO customers VALUES (7, 'David Miller');
INSERT INTO customers VALUES (8, 'Susan Moore');
INSERT INTO customers VALUES (9, 'Robert Taylor');
INSERT INTO customers VALUES (10, 'Patricia Anderson');

INSERT INTO accounts VALUES (1001, 1, 50000.00, 1);
INSERT INTO accounts VALUES (1002, 2, 200000.00, 1);
INSERT INTO accounts VALUES (1003, 3, 75000.00, 2);
INSERT INTO accounts VALUES (1004, 4, 10000.00, 2);
INSERT INTO accounts VALUES (1005, 5, 250000.00, 3);
INSERT INTO accounts VALUES (1006, 6, 34000.00, 3);
INSERT INTO accounts VALUES (1007, 7, 55000.00, 4);
INSERT INTO accounts VALUES (1008, 8, 80000.00, 4);
INSERT INTO accounts VALUES (1009, 9, 62000.00, 5);
INSERT INTO accounts VALUES (1010, 10, 91000.00, 5);

-- Seed Data for E-commerce Domain
INSERT INTO ecommerce_customers VALUES (1, 'Alice Jackson');
INSERT INTO ecommerce_customers VALUES (2, 'Brian Thomas');
INSERT INTO ecommerce_customers VALUES (3, 'Carol White');
INSERT INTO ecommerce_customers VALUES (4, 'Derek Harris');
INSERT INTO ecommerce_customers VALUES (5, 'Eva Martin');
INSERT INTO ecommerce_customers VALUES (6, 'Frank Garcia');
INSERT INTO ecommerce_customers VALUES (7, 'Grace Lee');
INSERT INTO ecommerce_customers VALUES (8, 'Henry Walker');
INSERT INTO ecommerce_customers VALUES (9, 'Ivy Hall');
INSERT INTO ecommerce_customers VALUES (10, 'Jack Young');

INSERT INTO products VALUES (1, 'Bluetooth Speaker', 'Electronics', 1999.00, FALSE);
INSERT INTO products VALUES (2, 'Running Shoes', 'Sports', 2999.00, FALSE);
INSERT INTO products VALUES (3, 'Smartphone', 'Electronics', 24999.00, FALSE);
INSERT INTO products VALUES (4, 'Cookware Set', 'Home', 3499.00, FALSE);
INSERT INTO products VALUES (5, 'T-shirt', 'Clothing', 499.00, FALSE);
INSERT INTO products VALUES (6, 'Wireless Mouse', 'Electronics', 999.00, TRUE);
INSERT INTO products VALUES (7, 'Office Chair', 'Home', 4999.00, FALSE);
INSERT INTO products VALUES (8, 'Book: SQL Basics', 'Books', 399.00, FALSE);
INSERT INTO products VALUES (9, 'Fitness Band', 'Electronics', 1999.00, FALSE);
INSERT INTO products VALUES (10, 'Football', 'Sports', 799.00, FALSE);

INSERT INTO orders VALUES (1, 1, 3, 1, 'Pending');
INSERT INTO orders VALUES (2, 2, 2, 2, 'Shipped');
INSERT INTO orders VALUES (3, 3, 1, 1, 'Delivered');
INSERT INTO orders VALUES (4, 4, 4, 1, 'Pending');
INSERT INTO orders VALUES (5, 5, 6, 3, 'Cancelled');
INSERT INTO orders VALUES (6, 6, 5, 2, 'Delivered');
INSERT INTO orders VALUES (7, 7, 7, 1, 'Pending');
INSERT INTO orders VALUES (8, 8, 9, 1, 'Shipped');
INSERT INTO orders VALUES (9, 9, 8, 1, 'Delivered');
INSERT INTO orders VALUES (10, 10, 10, 2, 'Pending');



Sure! Here's a **detailed guide to SQL topics**, with **each topic explained thoroughly**, followed by **two examples**:

1. **Banking domain** example
2. **E-commerce domain** example

This will help you gain both **domain-specific SQL insights** and strong **foundational SQL knowledge**.

---

## 1. **SQL Basics (SELECT, FROM, WHERE)**

### 🔹 Explanation:

These are the foundational commands to retrieve data:

* `SELECT`: Specifies which columns you want to retrieve.
* `FROM`: Specifies which table to get the data from.
* `WHERE`: Filters rows based on conditions.

---

### 🔸 Banking Example:

```sql
SELECT account_number, balance
FROM accounts
WHERE balance > 100000;
```

**Use Case**: Retrieve all accounts with a balance greater than ₹1,00,000 for premium banking services.

---

### 🔸 E-commerce Example:

```sql
SELECT product_name, price
FROM products
WHERE category = 'Electronics';
```

**Use Case**: Get a list of electronic products for display in the Electronics section of the site.

---

## 2. **SQL JOINs (INNER, LEFT, RIGHT, FULL)**

### 🔹 Explanation:

* **INNER JOIN**: Only matching rows in both tables.
* **LEFT JOIN**: All rows from the left table + matching from right.
* **RIGHT JOIN**: All from right + matching from left.
* **FULL JOIN**: All rows from both sides.

---

### 🔸 Banking Example:

```sql
SELECT c.customer_name, a.account_number, a.balance
FROM customers c
INNER JOIN accounts a ON c.customer_id = a.customer_id;
```

**Use Case**: Get customer details along with their account info.

---

### 🔸 E-commerce Example:

```sql
SELECT o.order_id, p.product_name, o.quantity
FROM orders o
LEFT JOIN products p ON o.product_id = p.product_id;
```

**Use Case**: Display orders even if some products are discontinued or missing.

---

## 3. **GROUP BY and Aggregation (SUM, COUNT, AVG, MAX, MIN)**

### 🔹 Explanation:

* Used to **group rows** that have the same values in specified columns.
* Aggregates like `SUM`, `AVG` compute summary statistics for each group.

---

### 🔸 Banking Example:

```sql
SELECT branch_id, SUM(balance) AS total_branch_balance
FROM accounts
GROUP BY branch_id;
```

**Use Case**: Calculate total deposits in each bank branch.

---

### 🔸 E-commerce Example:

```sql
SELECT customer_id, COUNT(*) AS total_orders
FROM orders
GROUP BY customer_id;
```

**Use Case**: Find how many orders each customer has placed.

---

## 4. **ORDER BY Clause**

### 🔹 Explanation:

Used to **sort** the result set by one or more columns, either **ASC** (ascending) or **DESC** (descending).

---

### 🔸 Banking Example:

```sql
SELECT account_number, balance
FROM accounts
ORDER BY balance DESC;
```

**Use Case**: Show top accounts by balance for internal audits.

---

### 🔸 E-commerce Example:

```sql
SELECT product_name, price
FROM products
ORDER BY price ASC;
```

**Use Case**: Display products sorted from cheapest to most expensive.

---

## 5. **Subqueries**

### 🔹 Explanation:

A subquery is a **query within a query**, used to filter or process data dynamically.

---

### 🔸 Banking Example:

```sql
SELECT customer_name
FROM customers
WHERE customer_id IN (
    SELECT customer_id
    FROM accounts
    WHERE balance > 100000
);
```

**Use Case**: Find customers with high-value accounts.

---

### 🔸 E-commerce Example:

```sql
SELECT product_name
FROM products
WHERE product_id IN (
    SELECT product_id
    FROM orders
    WHERE quantity > 5
);
```

**Use Case**: List products that are frequently bought in bulk.

---

## 6. **INSERT INTO**

### 🔹 Explanation:

Used to **add new rows** into a table.

---

### 🔸 Banking Example:

```sql
INSERT INTO accounts (account_number, customer_id, balance)
VALUES (1002001, 5, 50000);
```

**Use Case**: Add a new account for a customer.

---

### 🔸 E-commerce Example:

```sql
INSERT INTO products (product_id, product_name, price)
VALUES (2001, 'Bluetooth Speaker', 1999);
```

**Use Case**: Add a new product to the catalog.

---

## 7. **UPDATE Statement**

### 🔹 Explanation:

Used to **modify** existing records in a table.

---

### 🔸 Banking Example:

```sql
UPDATE accounts
SET balance = balance + 1000
WHERE account_number = 1002001;
```

**Use Case**: Add interest or promotional credit.

---

### 🔸 E-commerce Example:

```sql
UPDATE products
SET price = price * 0.9
WHERE category = 'Electronics';
```

**Use Case**: Apply a 10% discount on electronics.

---

## 8. **DELETE Statement**

### 🔹 Explanation:

Removes rows from a table based on condition.

---

### 🔸 Banking Example:

```sql
DELETE FROM accounts
WHERE balance = 0;
```

**Use Case**: Clean up inactive zero-balance accounts.

---

### 🔸 E-commerce Example:

```sql
DELETE FROM products
WHERE discontinued = TRUE;
```

**Use Case**: Remove discontinued products.

---

## 9. **SQL Constraints (PRIMARY KEY, FOREIGN KEY, UNIQUE, NOT NULL)**

### 🔹 Explanation:

Used to define rules for table columns:

* **PRIMARY KEY**: Uniquely identifies a row.
* **FOREIGN KEY**: Links two tables.
* **UNIQUE**: Ensures unique values.
* **NOT NULL**: Field must not be empty.

---

### 🔸 Banking Example:

```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);
```

**Use Case**: Ensure each customer has a unique ID and name must be present.

---

### 🔸 E-commerce Example:

```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```

**Use Case**: Ensure orders link correctly to customer records.

---

## 10. **SQL Views**

### 🔹 Explanation:

A view is a **virtual table** created by a query. It simplifies complex joins or filters.

---

### 🔸 Banking Example:

```sql
CREATE VIEW high_value_accounts AS
SELECT account_number, balance
FROM accounts
WHERE balance > 500000;
```

**Use Case**: Create a reusable summary for HNI accounts.

---

### 🔸 E-commerce Example:

```sql
CREATE VIEW active_orders AS
SELECT * FROM orders
WHERE status = 'Pending';
```

**Use Case**: Quickly access pending orders.

---

Would you like me to continue with advanced topics like **Stored Procedures, Triggers, Indexes, Transactions, Normalization**, etc. with examples from banking and e-commerce?



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

Excellent question — comparing `@Transactional` vs **stored procedures** in Spring Boot is essential for deciding **where** to put your business logic: in the **Java application** or in the **database**.

Let’s break it down clearly:

---

## ⚖️ `@Transactional` vs Stored Procedure

| Feature                           | `@Transactional` (Spring Boot)               | Stored Procedure (Database)                   |
| --------------------------------- | -------------------------------------------- | --------------------------------------------- |
| **Where logic resides**           | Java code (Service layer)                    | Database (SQL)                                |
| **Transaction management**        | Handled by Spring (`@Transactional`)         | Handled natively by DB (via `BEGIN/COMMIT`)   |
| **Readability / Maintainability** | Easier for Java developers                   | Requires DB/SQL knowledge                     |
| **Portability**                   | Portable across databases                    | Tied to specific DB (MySQL, Oracle, etc.)     |
| **Performance**                   | More network round-trips for each query      | Fast — logic runs on DB server                |
| **Debugging**                     | Easier with Java IDE and logs                | Harder to debug complex SQL                   |
| **Security**                      | More secure when sensitive logic stays in DB | Depends on user privileges and access control |
| **Scalability**                   | Depends on app performance                   | Great for heavy data tasks (less app load)    |
| **Version control & testing**     | Easy to manage with code repos, unit tests   | Harder to version/test stored procs           |

---

## ✅ When to Use `@Transactional` in Spring Boot

Use it when:

* You need full control of transaction boundaries in Java code.
* You're doing multiple related DB operations (CRUD) in a service layer.
* Logic is tightly coupled with your domain model or business objects.
* You want clean unit testing and mockability.

### Example:

```java
@Transactional
public void transferMoney(Long fromAcc, Long toAcc, BigDecimal amount) {
    Account a1 = accountRepository.findById(fromAcc).get();
    Account a2 = accountRepository.findById(toAcc).get();

    a1.setBalance(a1.getBalance().subtract(amount));
    a2.setBalance(a2.getBalance().add(amount));

    accountRepository.save(a1);
    accountRepository.save(a2);
}
```

---

## ✅ When to Use Stored Procedures

Use them when:

* You need **high performance** for large datasets or bulk processing.
* Logic is **data-centric** and doesn’t depend on app-side models.
* You want to **centralize business rules** inside the database.
* You need to **enforce rules at the DB level** across multiple applications.

### Example:

```sql
CREATE PROCEDURE transfer_money(
  IN from_acc BIGINT,
  IN to_acc BIGINT,
  IN amount DECIMAL(10,2)
)
BEGIN
  START TRANSACTION;
    UPDATE accounts SET balance = balance - amount WHERE id = from_acc;
    UPDATE accounts SET balance = balance + amount WHERE id = to_acc;
  COMMIT;
END;
```

---

## 🤔 Which One Should You Use?

| Situation                                 | Recommendation       |
| ----------------------------------------- | -------------------- |
| Need performance for heavy DB operations  | **Stored Procedure** |
| App needs full testing and flexibility    | **@Transactional**   |
| Logic shared across apps (DB-level rules) | **Stored Procedure** |
| Logic tied closely to Java models         | **@Transactional**   |
| Dev team stronger in Java than SQL        | **@Transactional**   |

---

## 💡 Best of Both?

You can **combine them**:

* Use `@Transactional` to wrap **Java-side orchestration**.
* Call **stored procedures** inside a service when performance matters.

---

Implementing **stored procedures in Spring Boot** is straightforward using **Spring Data JPA**. You can use either:

1. **@Procedure annotation** in your **JPA repository**, or
2. **EntityManager** for more flexible, custom calls.

---

## ✅ Use Case Example: Call a Stored Procedure in Spring Boot

Let’s assume you already have a stored procedure in MySQL:

### 🔁 MySQL Stored Procedure

```sql
DELIMITER $$

CREATE PROCEDURE sp_get_employees_by_department(IN dept_id INT)
BEGIN
  SELECT * FROM employees WHERE department_id = dept_id;
END$$

DELIMITER ;
```

---

## 🛠 Step-by-Step: Call Stored Procedure in Spring Boot

---

### 1️⃣ **Entity Class** (`Employee.java`)

```java
@Entity
@Table(name = "employees")
public class Employee {

    @Id
    private Long id;

    private String name;

    private Integer departmentId;

    // Getters and Setters
}
```

---

### 2️⃣ **Repository Interface Using @Procedure**

```java
public interface EmployeeRepository extends JpaRepository<Employee, Long> {

    // Option 1: If the stored procedure returns a list of employees
    @Procedure(name = "Employee.getEmployeesByDepartment")
    List<Employee> getEmployeesByDepartment(@Param("dept_id") Integer deptId);

}
```

To use `@Procedure` like this, you also need to map the procedure name using `@NamedStoredProcedureQuery`.

---

### 3️⃣ **Update Entity with Stored Procedure Mapping**

```java
@NamedStoredProcedureQuery(
    name = "Employee.getEmployeesByDepartment",
    procedureName = "sp_get_employees_by_department",
    parameters = {
        @StoredProcedureParameter(mode = ParameterMode.IN, name = "dept_id", type = Integer.class)
    },
    resultClasses = Employee.class
)
@Entity
@Table(name = "employees")
public class Employee {
    // Same as above
}
```

---

### 4️⃣ **Use It in a Service Layer**

```java
@Service
public class EmployeeService {

    @Autowired
    private EmployeeRepository employeeRepository;

    public List<Employee> getEmployeesByDepartment(Integer deptId) {
        return employeeRepository.getEmployeesByDepartment(deptId);
    }
}
```

---

### 5️⃣ **Call It from a Controller**

```java
@RestController
@RequestMapping("/api/employees")
public class EmployeeController {

    @Autowired
    private EmployeeService employeeService;

    @GetMapping("/department/{id}")
    public List<Employee> getByDepartment(@PathVariable Integer id) {
        return employeeService.getEmployeesByDepartment(id);
    }
}
```

---

## 🧰 Alternative: Use `EntityManager` (More Flexibility)

If your stored procedure has OUT parameters, complex logic, or doesn't map directly to an entity, use `EntityManager`:

```java
@Autowired
private EntityManager entityManager;

public List<Employee> callStoredProcedure(Integer deptId) {
    StoredProcedureQuery query = entityManager
        .createStoredProcedureQuery("sp_get_employees_by_department", Employee.class)
        .registerStoredProcedureParameter("dept_id", Integer.class, ParameterMode.IN)
        .setParameter("dept_id", deptId);

    return query.getResultList();
}
```

---

## 📦 Summary

| Feature                          | Approach                          |
| -------------------------------- | --------------------------------- |
| Simple result mapping            | `@Procedure` with JPA annotations |
| Multiple input/output parameters | Use `EntityManager`               |
| Named procedure in entity class  | `@NamedStoredProcedureQuery`      |

---

Great! Here's how you can:

* ✅ Create **Materialized Views** for both **Banking** and **E-commerce** domains
* ✅ Set them to **refresh once per day**

> ⚠️ Note: Support for **materialized views** and **scheduled refresh** depends on the **database system** (e.g., PostgreSQL, Oracle, MySQL, SQL Server). Below, I provide implementations for:

---

## 🔹 1. PostgreSQL: Materialized Views + Daily Refresh via `cron` (pg\_cron)

### ✅ Banking: High Value Accounts

```sql
CREATE MATERIALIZED VIEW mv_high_value_accounts AS
SELECT account_number, customer_id, balance
FROM accounts
WHERE balance > 100000
WITH DATA;
```

### ✅ E-commerce: Daily Sales Summary

```sql
CREATE MATERIALIZED VIEW mv_daily_order_summary AS
SELECT customer_id, COUNT(*) AS total_orders, SUM(quantity) AS total_items
FROM orders
GROUP BY customer_id
WITH DATA;
```

### 🔁 Schedule Daily Refresh using `pg_cron`

1. Install the `pg_cron` extension (if not already):

```sql
CREATE EXTENSION IF NOT EXISTS pg_cron;
```

2. Schedule a daily refresh (e.g., 2 AM):

```sql
SELECT cron.schedule('refresh_mv_accounts', '0 2 * * *', $$ REFRESH MATERIALIZED VIEW mv_high_value_accounts; $$);
SELECT cron.schedule('refresh_mv_orders', '0 2 * * *', $$ REFRESH MATERIALIZED VIEW mv_daily_order_summary; $$);
```

---

## 🔹 2. Oracle: Materialized Views + Automatic Refresh

### ✅ Banking: High Value Accounts

```sql
CREATE MATERIALIZED VIEW mv_high_value_accounts
BUILD IMMEDIATE
REFRESH COMPLETE
START WITH SYSDATE
NEXT SYSDATE + 1
AS
SELECT account_number, customer_id, balance
FROM accounts
WHERE balance > 100000;
```

### ✅ E-commerce: Daily Orders Summary

```sql
CREATE MATERIALIZED VIEW mv_daily_order_summary
BUILD IMMEDIATE
REFRESH COMPLETE
START WITH SYSDATE
NEXT SYSDATE + 1
AS
SELECT customer_id, COUNT(*) AS total_orders, SUM(quantity) AS total_items
FROM orders
GROUP BY customer_id;
```

Oracle automatically refreshes based on `START WITH` and `NEXT` clauses.

---

## 🔹 3. MySQL: Emulated Materialized Views via Table + Event Scheduler

MySQL doesn’t support materialized views natively. You simulate them:

### Step 1: Create Table to Hold Data

```sql
CREATE TABLE mv_high_value_accounts AS
SELECT account_number, customer_id, balance
FROM accounts
WHERE balance > 100000;

CREATE TABLE mv_daily_order_summary AS
SELECT customer_id, COUNT(*) AS total_orders, SUM(quantity) AS total_items
FROM orders
GROUP BY customer_id;
```

### Step 2: Create SQL Event to Refresh Daily

```sql
-- Enable Event Scheduler
SET GLOBAL event_scheduler = ON;

-- Event for Banking
CREATE EVENT refresh_mv_high_value_accounts
ON SCHEDULE EVERY 1 DAY
DO
  REPLACE INTO mv_high_value_accounts
  SELECT account_number, customer_id, balance
  FROM accounts
  WHERE balance > 100000;

-- Event for E-commerce
CREATE EVENT refresh_mv_daily_order_summary
ON SCHEDULE EVERY 1 DAY
DO
  REPLACE INTO mv_daily_order_summary
  SELECT customer_id, COUNT(*) AS total_orders, SUM(quantity) AS total_items
  FROM orders
  GROUP BY customer_id;
```

---

## ✅ Summary

| DBMS       | Materialized View | Auto Refresh    |
| ---------- | ----------------- | --------------- |
| PostgreSQL | ✅ Supported       | ✅ via `pg_cron` |
| Oracle     | ✅ Supported       | ✅ with built-in |
| MySQL      | ❌ (emulated)      | ✅ with Events   |

---
Certainly! Below are **5 SQL queries using core subqueries** based on the **banking** and **e-commerce** schema we created earlier. These include various types of subqueries such as **scalar**, **correlated**, and **IN/EXISTS** subqueries.

---

## 🔹 1. **Banking** — Find customers who have at least one account with a balance above the average balance

### 📘 Type: Correlated Subquery

```sql
SELECT customer_name
FROM customers
WHERE customer_id IN (
    SELECT customer_id
    FROM accounts
    WHERE balance > (
        SELECT AVG(balance)
        FROM accounts
    )
);
```

📌 **Explanation**: Subquery calculates the average balance. Outer query finds customers whose **account balance is greater than this average**.

---

## 🔹 2. **E-commerce** — List products that were ordered at least once

### 📘 Type: IN Subquery

```sql
SELECT product_name
FROM products
WHERE product_id IN (
    SELECT DISTINCT product_id
    FROM orders
);
```

📌 **Explanation**: Inner query pulls all product IDs from the `orders` table. Outer query filters products that exist in at least one order.

---

## 🔹 3. **Banking** — Find the account with the highest balance per branch

### 📘 Type: Correlated Subquery

```sql
SELECT account_number, branch_id, balance
FROM accounts a
WHERE balance = (
    SELECT MAX(balance)
    FROM accounts
    WHERE branch_id = a.branch_id
);
```

📌 **Explanation**: For each account, subquery gets the max balance **within that same branch**.

---

## 🔹 4. **E-commerce** — Find customers who ordered the most expensive product

### 📘 Type: Scalar Subquery

```sql
SELECT DISTINCT c.customer_name
FROM ecommerce_customers c
JOIN orders o ON c.customer_id = o.customer_id
WHERE o.product_id = (
    SELECT product_id
    FROM products
    ORDER BY price DESC
    LIMIT 1
);
```

📌 **Explanation**: The subquery finds the product with the highest price. Main query returns customers who ordered it.

---

## 🔹 5. **Banking** — Show accounts whose balance is greater than the total balance of customer ID 1

### 📘 Type: Scalar Subquery

```sql
SELECT account_number, balance
FROM accounts
WHERE balance > (
    SELECT SUM(balance)
    FROM accounts
    WHERE customer_id = 1
);
```

📌 **Explanation**: Subquery sums up all balances of customer 1. The outer query finds accounts **with more than that**.

---

### ✅ Summary of Subquery Types Used:

| # | Query Description                                | Subquery Type   |
| - | ------------------------------------------------ | --------------- |
| 1 | Customers with above-average balance accounts    | Correlated      |
| 2 | Products that were ever ordered                  | IN Subquery     |
| 3 | Max balance per branch                           | Correlated      |
| 4 | Customers who ordered the most expensive product | Scalar Subquery |
| 5 | Accounts with balance > total of customer ID 1   | Scalar Subquery |

---

Excellent choice! **Subqueries**, especially correlated ones, can be less efficient than **JOINs**, especially on large datasets. Below, I’ve converted all 5 subqueries to their **JOIN-based equivalents** while maintaining the original logic and improving performance.

---

## 🔁 1. **Banking** — Customers with accounts above average balance

### ✅ Original (Subquery):

```sql
SELECT customer_name
FROM customers
WHERE customer_id IN (
    SELECT customer_id
    FROM accounts
    WHERE balance > (
        SELECT AVG(balance) FROM accounts
    )
);
```

### 🔄 Converted (JOIN):

```sql
SELECT DISTINCT c.customer_name
FROM customers c
JOIN accounts a ON c.customer_id = a.customer_id
JOIN (
    SELECT AVG(balance) AS avg_balance
    FROM accounts
) avg_sub ON a.balance > avg_sub.avg_balance;
```

---

## 🔁 2. **E-commerce** — Products that were ordered at least once

### ✅ Original (Subquery):

```sql
SELECT product_name
FROM products
WHERE product_id IN (
    SELECT DISTINCT product_id FROM orders
);
```

### 🔄 Converted (JOIN):

```sql
SELECT DISTINCT p.product_name
FROM products p
JOIN orders o ON p.product_id = o.product_id;
```

---

## 🔁 3. **Banking** — Accounts with max balance per branch

### ✅ Original (Subquery):

```sql
SELECT account_number, branch_id, balance
FROM accounts a
WHERE balance = (
    SELECT MAX(balance)
    FROM accounts
    WHERE branch_id = a.branch_id
);
```

### 🔄 Converted (JOIN):

```sql
SELECT a.account_number, a.branch_id, a.balance
FROM accounts a
JOIN (
    SELECT branch_id, MAX(balance) AS max_balance
    FROM accounts
    GROUP BY branch_id
) b_max ON a.branch_id = b_max.branch_id AND a.balance = b_max.max_balance;
```

---

## 🔁 4. **E-commerce** — Customers who ordered the most expensive product

### ✅ Original (Subquery):

```sql
SELECT DISTINCT c.customer_name
FROM ecommerce_customers c
JOIN orders o ON c.customer_id = o.customer_id
WHERE o.product_id = (
    SELECT product_id
    FROM products
    ORDER BY price DESC
    LIMIT 1
);
```

### 🔄 Converted (JOIN):

```sql
-- PostgreSQL / MySQL 8+ with CTE & ROW_NUMBER for top product
WITH top_product AS (
    SELECT product_id
    FROM products
    ORDER BY price DESC
    LIMIT 1
)
SELECT DISTINCT c.customer_name
FROM ecommerce_customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN top_product tp ON o.product_id = tp.product_id;
```

---

## 🔁 5. **Banking** — Accounts with balance > total balance of customer 1

### ✅ Original (Subquery):

```sql
SELECT account_number, balance
FROM accounts
WHERE balance > (
    SELECT SUM(balance)
    FROM accounts
    WHERE customer_id = 1
);
```

### 🔄 Converted (JOIN):

```sql
SELECT a.account_number, a.balance
FROM accounts a
JOIN (
    SELECT SUM(balance) AS total_balance
    FROM accounts
    WHERE customer_id = 1
) cust1_total ON a.balance > cust1_total.total_balance;
```

---

## ✅ Summary Table: Subqueries Converted to JOINs

| # | Description                                  | Benefit of JOIN                |
| - | -------------------------------------------- | ------------------------------ |
| 1 | Above-average account balance                | Avoids nested aggregation      |
| 2 | Products ever ordered                        | Simple inner join              |
| 3 | Max balance per branch                       | Avoids correlated subquery     |
| 4 | Customers who ordered most expensive product | Easier to index/join           |
| 5 | Balance > total of one customer              | More efficient via scalar JOIN |

---

Would you like me to package these optimized JOIN queries into a downloadable `.sql` file?
