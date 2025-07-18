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

Absolutely! Let's walk through **SQL JOINs** in detail using your schema for the **Banking** and **E-commerce** domains.

---

## 🔰 What is a JOIN?

A **JOIN** combines rows from two or more tables based on a related column. It allows you to **query across multiple tables** and retrieve meaningful combined results.

---

## 🔗 Types of SQL Joins (with Explanation & Examples):

| Type       | Description                                                       |
| ---------- | ----------------------------------------------------------------- |
| INNER JOIN | Returns rows where there is a match in both tables                |
| LEFT JOIN  | Returns all rows from the left table and matching ones from right |
| RIGHT JOIN | Returns all rows from the right table and matching ones from left |
| FULL OUTER | Returns all rows when there is a match in one of the tables       |
| CROSS JOIN | Returns **Cartesian product** of both tables                      |
| SELF JOIN  | Join the same table with itself                                   |

---

### 📘 Your Schema Reference

#### Banking Tables:

* `customers(customer_id, customer_name)`
* `accounts(account_number, customer_id, balance, branch_id)`

#### E-commerce Tables:

* `ecommerce_customers(customer_id, customer_name)`
* `orders(order_id, customer_id, product_id, quantity, status)`
* `products(product_id, product_name, category, price, discontinued)`

---

## 1. 🔸 INNER JOIN

### ✅ Goal: List customers with their accounts (only if they have accounts)

```sql
SELECT c.customer_name, a.account_number, a.balance
FROM customers c
INNER JOIN accounts a ON c.customer_id = a.customer_id;
```

### 🔍 Explanation:

* Combines only those customers **who have at least one account**.
* Excludes customers without accounts.

---

## 2. 🔸 LEFT JOIN

### ✅ Goal: List all customers, including those without any accounts

```sql
SELECT c.customer_name, a.account_number, a.balance
FROM customers c
LEFT JOIN accounts a ON c.customer_id = a.customer_id;
```

### 🔍 Explanation:

* Returns **all customers**.
* If a customer has no account, `account_number` and `balance` will be **NULL**.

---

## 3. 🔸 RIGHT JOIN

> Not supported in all databases (e.g., MySQL supports it; PostgreSQL prefers flipping LEFT JOIN).

### ✅ Goal: List all accounts and their customer info (even if orphaned)

```sql
SELECT c.customer_name, a.account_number, a.balance
FROM customers c
RIGHT JOIN accounts a ON c.customer_id = a.customer_id;
```

### 🔍 Explanation:

* Returns **all accounts**, even if the customer data is missing (uncommon in normalized schema).

---

## 4. 🔸 FULL OUTER JOIN

> Supported in PostgreSQL, SQL Server. Not directly in MySQL.

### ✅ Goal: Show all customer-account pairs, even if either side is missing.

```sql
SELECT c.customer_name, a.account_number, a.balance
FROM customers c
FULL OUTER JOIN accounts a ON c.customer_id = a.customer_id;
```

### 🔍 Explanation:

* Includes:

  * All customers (with or without accounts)
  * All accounts (with or without customers)

---

## 5. 🔸 CROSS JOIN

### ✅ Goal: Show every combination of product and order (not common unless intended)

```sql
SELECT p.product_name, o.order_id
FROM products p
CROSS JOIN orders o;
```

### 🔍 Explanation:

* Returns a **Cartesian product**:

  * Every product paired with every order.
* Useful in generating combinations or testing.

---

## 6. 🔸 SELF JOIN

### ✅ Goal: Find pairs of customers in the same branch (imaginary case)

```sql
SELECT a1.account_number AS acc1, a2.account_number AS acc2, a1.branch_id
FROM accounts a1
JOIN accounts a2 ON a1.branch_id = a2.branch_id
WHERE a1.account_number < a2.account_number;
```

### 🔍 Explanation:

* Joins `accounts` table with itself to compare accounts in the same branch.

---

## 📊 Visual Representation of JOIN Types

```
Table A: customers
Table B: accounts

INNER JOIN    = A ∩ B
LEFT JOIN     = All A + matching B
RIGHT JOIN    = All B + matching A
FULL OUTER    = A ∪ B
CROSS JOIN    = All combinations (A × B)
SELF JOIN     = A joined to A
```

---

## ✅ JOINs in E-Commerce Domain

### Example: Orders with product and customer info

```sql
SELECT ec.customer_name, p.product_name, o.quantity, o.status
FROM orders o
JOIN ecommerce_customers ec ON o.customer_id = ec.customer_id
JOIN products p ON o.product_id = p.product_id;
```

---

## 🧠 Tips for Using JOINS

* Use **aliases** (`c`, `a`, etc.) to improve readability.
* Always qualify columns when working across tables.
* Use **`LEFT JOIN`** when data may not exist in the joined table.
* Use **`INNER JOIN`** when you only want matching rows.

---
-- USERS
INSERT INTO users (user_id, name, email, created_at) VALUES
(1, 'Alice Johnson', 'alice@example.com', '2024-01-15 10:00:00'),
(2, 'Bob Smith', 'bob@example.com', '2024-02-20 09:30:00'),
(3, 'Charlie Brown', 'charlie@example.com', '2024-03-05 12:45:00');

-- CATEGORIES
INSERT INTO categories (category_id, category_name) VALUES
(1, 'Electronics'),
(2, 'Books'),
(3, 'Clothing');

-- PRODUCTS
INSERT INTO products (product_id, name, category_id, price, stock_quantity) VALUES
(1, 'Smartphone', 1, 699.99, 50),
(2, 'Laptop', 1, 999.99, 30),
(3, 'Novel', 2, 15.99, 200),
(4, 'T-shirt', 3, 9.99, 100);

-- ORDERS
INSERT INTO orders (order_id, user_id, order_date, status, total_amount) VALUES
(1, 1, '2024-06-01 14:00:00', 'Shipped', 709.98),
(2, 2, '2024-06-05 16:20:00', 'Pending', 15.99),
(3, 1, '2024-06-07 11:00:00', 'Cancelled', 1009.98);

-- ORDER_ITEMS
INSERT INTO order_items (order_item_id, order_id, product_id, quantity, unit_price) VALUES
(1, 1, 1, 1, 699.99),
(2, 1, 4, 1, 9.99),
(3, 2, 3, 1, 15.99),
(4, 3, 2, 1, 999.99),
(5, 3, 4, 1, 9.99);

-- PAYMENTS
INSERT INTO payments (payment_id, order_id, payment_method, payment_date, amount) VALUES
(1, 1, 'Credit Card', '2024-06-01 14:05:00', 709.98),
(2, 2, 'PayPal', '2024-06-05 16:25:00', 15.99),
(3, 3, 'Credit Card', '2024-06-07 11:10:00', 1009.98);

-- ADDRESSES
INSERT INTO addresses (address_id, user_id, city, state, zip_code, country) VALUES
(1, 1, 'New York', 'NY', '10001', 'USA'),
(2, 2, 'Los Angeles', 'CA', '90001', 'USA'),
(3, 3, 'Chicago', 'IL', '60601', 'USA');

-- REVIEWS
INSERT INTO reviews (review_id, user_id, product_id, rating, review_text, review_date) VALUES
(1, 1, 1, 5, 'Excellent smartphone!', '2024-06-10 10:00:00'),
(2, 2, 3, 4, 'Great read, loved the characters.', '2024-06-11 15:30:00'),
(3, 1, 4, 3, 'Average quality t-shirt.', '2024-06-12 09:00:00');

-- CART
INSERT INTO cart (cart_id, user_id, created_at) VALUES
(1, 1, '2024-06-15 10:00:00'),
(2, 2, '2024-06-16 11:00:00');

-- CART_ITEMS
INSERT INTO cart_items (cart_item_id, cart_id, product_id, quantity) VALUES
(1, 1, 2, 1),
(2, 1, 3, 2),
(3, 2, 4, 3);


Great! Here's a **complete SQL schema script** for the **e-commerce database** with the 10 tables we discussed, including primary keys, foreign keys, and relevant data types. This is designed to work with **PostgreSQL** or **MySQL** (minor tweaks may be needed between them).

---

## 🗂️ **E-Commerce SQL Schema Script (10 Tables)**

```sql
-- USERS
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- CATEGORIES
CREATE TABLE categories (
    category_id SERIAL PRIMARY KEY,
    category_name VARCHAR(100) NOT NULL
);

-- PRODUCTS
CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    category_id INT,
    price DECIMAL(10, 2),
    stock_quantity INT,
    FOREIGN KEY (category_id) REFERENCES categories(category_id)
);

-- ORDERS
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    user_id INT,
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    status VARCHAR(50),
    total_amount DECIMAL(10, 2),
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);

-- ORDER_ITEMS
CREATE TABLE order_items (
    order_item_id SERIAL PRIMARY KEY,
    order_id INT,
    product_id INT,
    quantity INT,
    unit_price DECIMAL(10, 2),
    FOREIGN KEY (order_id) REFERENCES orders(order_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);

-- PAYMENTS
CREATE TABLE payments (
    payment_id SERIAL PRIMARY KEY,
    order_id INT,
    payment_method VARCHAR(50),
    payment_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    amount DECIMAL(10, 2),
    FOREIGN KEY (order_id) REFERENCES orders(order_id)
);

-- ADDRESSES
CREATE TABLE addresses (
    address_id SERIAL PRIMARY KEY,
    user_id INT,
    city VARCHAR(100),
    state VARCHAR(100),
    zip_code VARCHAR(20),
    country VARCHAR(100),
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);

-- REVIEWS
CREATE TABLE reviews (
    review_id SERIAL PRIMARY KEY,
    user_id INT,
    product_id INT,
    rating INT CHECK (rating BETWEEN 1 AND 5),
    review_text TEXT,
    review_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(user_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);

-- CART
CREATE TABLE cart (
    cart_id SERIAL PRIMARY KEY,
    user_id INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);

-- CART_ITEMS
CREATE TABLE cart_items (
    cart_item_id SERIAL PRIMARY KEY,
    cart_id INT,
    product_id INT,
    quantity INT,
    FOREIGN KEY (cart_id) REFERENCES cart(cart_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);
```

---

## ✅ Optional: Insert Sample Data

Would you like me to generate realistic sample data (20–50 rows per table) for this schema using `INSERT` statements or to provide a CSV/SQL file to download?

Let me know which format you’d prefer:

* SQL script with `INSERT` statements
* CSV files per table
* JSON format
* A downloadable `.sql` file

Also, specify whether you're using **PostgreSQL**, **MySQL**, or **SQLite**.
Sure! Here's a comprehensive list of **SQL queries across all major topics** in the SQL domain, using **10 interrelated tables** from an **e-commerce database schema**. This should serve as a practical reference or exercise set for intermediate to advanced SQL skills.

---

### 🗂️ **Schema Design (10 Tables)**

```plaintext
1. users            (user_id, name, email, created_at)
2. products         (product_id, name, category_id, price, stock_quantity)
3. categories       (category_id, category_name)
4. orders           (order_id, user_id, order_date, status, total_amount)
5. order_items      (order_item_id, order_id, product_id, quantity, unit_price)
6. payments         (payment_id, order_id, payment_method, payment_date, amount)
7. addresses        (address_id, user_id, city, state, zip_code, country)
8. reviews          (review_id, user_id, product_id, rating, review_text, review_date)
9. cart             (cart_id, user_id, created_at)
10. cart_items      (cart_item_id, cart_id, product_id, quantity)
```

---

### ✅ **1. Basic SELECT**

```sql
SELECT name, email FROM users;
```

### ✅ **2. Filtering with WHERE**

```sql
SELECT * FROM products WHERE price > 100 AND stock_quantity > 0;
```

### ✅ **3. JOIN (INNER, LEFT, RIGHT)**

```sql
-- Get order details with product names
SELECT o.order_id, p.name AS product_name, oi.quantity
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON p.product_id = oi.product_id;
```

### ✅ **4. GROUP BY + Aggregates**

```sql
-- Total spent by each user
SELECT u.user_id, u.name, SUM(o.total_amount) AS total_spent
FROM users u
JOIN orders o ON u.user_id = o.user_id
GROUP BY u.user_id, u.name;
```

### ✅ **5. Subqueries**

```sql
-- Users who made purchases above average order amount
SELECT user_id, name
FROM users
WHERE user_id IN (
    SELECT user_id FROM orders
    WHERE total_amount > (SELECT AVG(total_amount) FROM orders)
);
```

### ✅ **6. Window Functions**

```sql
-- Running total of payments by user
SELECT 
    user_id,
    order_id,
    amount,
    SUM(amount) OVER (PARTITION BY user_id ORDER BY payment_date) AS running_total
FROM orders o
JOIN payments p ON o.order_id = p.order_id;
```

### ✅ **7. CASE Statements**

```sql
-- Categorize products based on price range
SELECT name,
       price,
       CASE
         WHEN price < 50 THEN 'Low'
         WHEN price BETWEEN 50 AND 200 THEN 'Medium'
         ELSE 'High'
       END AS price_category
FROM products;
```

### ✅ **8. EXISTS / NOT EXISTS**

```sql
-- Products that have never been ordered
SELECT name
FROM products p
WHERE NOT EXISTS (
    SELECT 1 FROM order_items oi WHERE oi.product_id = p.product_id
);
```

### ✅ **9. Common Table Expressions (CTE)**

```sql
-- Get top selling products
WITH product_sales AS (
  SELECT product_id, SUM(quantity) AS total_sold
  FROM order_items
  GROUP BY product_id
)
SELECT p.name, ps.total_sold
FROM product_sales ps
JOIN products p ON ps.product_id = p.product_id
ORDER BY ps.total_sold DESC;
```

### ✅ **10. UNION / UNION ALL**

```sql
-- All product IDs from cart and order items
SELECT product_id FROM cart_items
UNION
SELECT product_id FROM order_items;
```

### ✅ **11. Aggregation with HAVING**

```sql
-- Users who placed more than 3 orders
SELECT user_id, COUNT(*) AS order_count
FROM orders
GROUP BY user_id
HAVING COUNT(*) > 3;
```

### ✅ **12. Self Join**

```sql
-- Users in the same city
SELECT a1.user_id AS user1, a2.user_id AS user2, a1.city
FROM addresses a1
JOIN addresses a2 ON a1.city = a2.city AND a1.user_id <> a2.user_id;
```

### ✅ **13. Updating with JOIN**

```sql
-- Reduce price of all products in category 'Electronics' by 10%
UPDATE products
SET price = price * 0.9
WHERE category_id IN (
    SELECT category_id FROM categories WHERE category_name = 'Electronics'
);
```

### ✅ **14. DELETE with Condition**

```sql
-- Delete inactive carts older than 30 days
DELETE FROM cart
WHERE created_at < NOW() - INTERVAL '30 days';
```

### ✅ **15. Advanced Filtering**

```sql
-- Orders with multiple items
SELECT order_id
FROM order_items
GROUP BY order_id
HAVING COUNT(*) > 1;
```

### ✅ **16. Correlated Subquery**

```sql
-- Products that cost more than the average for their category
SELECT name, price
FROM products p
WHERE price > (
    SELECT AVG(price) FROM products WHERE category_id = p.category_id
);
```

### ✅ **17. Top-N Queries**

```sql
-- Top 5 most expensive products
SELECT * FROM products
ORDER BY price DESC
LIMIT 5;
```

### ✅ **18. PIVOT-like Query (Conditional Aggregation)**

```sql
-- Total orders per status
SELECT
  COUNT(*) FILTER (WHERE status = 'Pending') AS pending,
  COUNT(*) FILTER (WHERE status = 'Shipped') AS shipped,
  COUNT(*) FILTER (WHERE status = 'Cancelled') AS cancelled
FROM orders;
```

### ✅ **19. Multi-table Aggregation**

```sql
-- Total revenue by category
SELECT c.category_name, SUM(oi.quantity * oi.unit_price) AS total_revenue
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
JOIN categories c ON p.category_id = c.category_id
GROUP BY c.category_name;
```

### ✅ **20. Nested CTEs + Window Functions**

```sql
-- Rank users by total spend and get top 3
WITH user_spend AS (
    SELECT u.user_id, u.name, SUM(o.total_amount) AS total_spent
    FROM users u
    JOIN orders o ON u.user_id = o.user_id
    GROUP BY u.user_id, u.name
),
ranked AS (
    SELECT *, RANK() OVER (ORDER BY total_spent DESC) AS rank
    FROM user_spend
)
SELECT * FROM ranked WHERE rank <= 3;
```

---

Absolutely! Here's a curated list of **20 advanced SQL queries** for the **e-commerce schema** you’re using. Each query involves **at least 5 tables**, along with a clear **business requirement**. These queries will help you understand multi-table joins, aggregations, filtering, subqueries, window functions, and business logic applications in SQL.

---

## ✅ **20 SQL Queries on E-Commerce Schema (5+ Tables Each)**

---

### 1. **Customer Order Summary with Reviews and Shipping Info**

**📌 Requirement**: For each customer, show all order details including products, review (if any), and shipping address.

```sql
SELECT 
    u.user_id, u.name, o.order_id, o.order_date, o.status,
    p.name AS product_name, oi.quantity, oi.unit_price,
    r.rating, r.review_text, a.city, a.state
FROM users u
JOIN orders o ON u.user_id = o.user_id
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id
LEFT JOIN reviews r ON u.user_id = r.user_id AND r.product_id = p.product_id
LEFT JOIN addresses a ON u.user_id = a.user_id;
```

---

### 2. **Top 5 Cities by Total Sales**

**📌 Requirement**: Find the top 5 cities with the highest total sales from completed orders.

```sql
SELECT 
    a.city, SUM(o.total_amount) AS total_sales
FROM orders o
JOIN users u ON o.user_id = u.user_id
JOIN addresses a ON u.user_id = a.user_id
WHERE o.status = 'Shipped'
GROUP BY a.city
ORDER BY total_sales DESC
LIMIT 5;
```

---

### 3. **Product Review & Purchase Correlation**

**📌 Requirement**: List all products that were purchased and reviewed by the same user.

```sql
SELECT DISTINCT
    u.user_id, u.name, p.name AS product_name,
    r.rating, r.review_text
FROM users u
JOIN orders o ON u.user_id = o.user_id
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id
JOIN reviews r ON r.user_id = u.user_id AND r.product_id = p.product_id;
```

---

### 4. **Cart-to-Order Conversion Rate per User**

**📌 Requirement**: For each user, check if products in cart were later ordered.

```sql
SELECT 
    u.user_id, u.name, p.name AS product_name,
    CASE WHEN oi.product_id IS NOT NULL THEN 'Ordered' ELSE 'Not Ordered' END AS order_status
FROM users u
JOIN cart c ON u.user_id = c.user_id
JOIN cart_items ci ON c.cart_id = ci.cart_id
JOIN products p ON ci.product_id = p.product_id
LEFT JOIN order_items oi ON p.product_id = oi.product_id
LEFT JOIN orders o ON oi.order_id = o.order_id AND o.user_id = u.user_id;
```

---

### 5. **Repeat Purchase Analysis**

**📌 Requirement**: List users who purchased the same product more than once in different orders.

```sql
SELECT 
    u.user_id, u.name, p.name AS product_name, COUNT(DISTINCT o.order_id) AS purchase_count
FROM users u
JOIN orders o ON u.user_id = o.user_id
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id
GROUP BY u.user_id, u.name, p.name
HAVING COUNT(DISTINCT o.order_id) > 1;
```

---

### 6. **Average Order Value by Product Category**

**📌 Requirement**: Calculate average order value per category, based on order items.

```sql
SELECT 
    c.category_name, AVG(oi.quantity * oi.unit_price) AS avg_order_value
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
JOIN categories c ON p.category_id = c.category_id
JOIN orders o ON oi.order_id = o.order_id
WHERE o.status = 'Shipped'
GROUP BY c.category_name;
```

---

### 7. **User's Complete Order & Review Trail**

**📌 Requirement**: For each user, show orders, items purchased, reviews, and payment method.

```sql
SELECT 
    u.name, o.order_id, p.name AS product_name, oi.quantity,
    r.rating, r.review_text, pay.payment_method
FROM users u
JOIN orders o ON u.user_id = o.user_id
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id
LEFT JOIN reviews r ON r.user_id = u.user_id AND r.product_id = p.product_id
JOIN payments pay ON o.order_id = pay.order_id;
```

---

### 8. **High-Value Customers Who Also Leave Reviews**

**📌 Requirement**: Identify users who spent over \$1000 and also reviewed at least 2 products.

```sql
SELECT 
    u.user_id, u.name, SUM(o.total_amount) AS total_spent, COUNT(DISTINCT r.review_id) AS reviews_given
FROM users u
JOIN orders o ON u.user_id = o.user_id
JOIN reviews r ON u.user_id = r.user_id
GROUP BY u.user_id, u.name
HAVING SUM(o.total_amount) > 1000 AND COUNT(DISTINCT r.review_id) >= 2;
```

---

### 9. **Most Reviewed Products by Category**

**📌 Requirement**: Show the most reviewed product in each category.

```sql
WITH review_counts AS (
    SELECT p.product_id, p.name, p.category_id, COUNT(r.review_id) AS review_count
    FROM products p
    LEFT JOIN reviews r ON p.product_id = r.product_id
    GROUP BY p.product_id, p.name, p.category_id
),
ranked_reviews AS (
    SELECT *, RANK() OVER (PARTITION BY category_id ORDER BY review_count DESC) AS rnk
    FROM review_counts
)
SELECT rc.name AS product_name, c.category_name, rc.review_count
FROM ranked_reviews rc
JOIN categories c ON rc.category_id = c.category_id
WHERE rnk = 1;
```

---

### 10. **Order Fulfillment Time Report**

**📌 Requirement**: Show time between order date and payment date for each order.

```sql
SELECT 
    u.name, o.order_id, o.order_date, p.payment_date,
    EXTRACT(EPOCH FROM (p.payment_date - o.order_date))/3600 AS hours_to_payment
FROM users u
JOIN orders o ON u.user_id = o.user_id
JOIN payments p ON o.order_id = p.order_id;
```

---

### 11. **Top Selling Product in Each City**

**📌 Requirement**: Find the best-selling product in terms of quantity for each city.

```sql
WITH sales_by_city AS (
    SELECT 
        a.city, p.product_id, p.name AS product_name, SUM(oi.quantity) AS total_sold
    FROM order_items oi
    JOIN orders o ON oi.order_id = o.order_id
    JOIN users u ON o.user_id = u.user_id
    JOIN addresses a ON u.user_id = a.user_id
    JOIN products p ON oi.product_id = p.product_id
    GROUP BY a.city, p.product_id, p.name
),
ranked_sales AS (
    SELECT *, RANK() OVER (PARTITION BY city ORDER BY total_sold DESC) AS rnk
    FROM sales_by_city
)
SELECT city, product_name, total_sold
FROM ranked_sales
WHERE rnk = 1;
```

---

### 12. **Users with Abandoned Carts and Later Purchases**

**📌 Requirement**: Identify users who added products to cart but didn’t order immediately, but purchased later.

```sql
SELECT DISTINCT u.user_id, u.name
FROM users u
JOIN cart c ON u.user_id = c.user_id
JOIN cart_items ci ON c.cart_id = ci.cart_id
JOIN products p ON ci.product_id = p.product_id
LEFT JOIN order_items oi ON p.product_id = oi.product_id
LEFT JOIN orders o ON oi.order_id = o.order_id AND o.user_id = u.user_id
WHERE o.order_date > c.created_at;
```

---

### 13. **Cross-Category Purchase Analysis**

**📌 Requirement**: Find users who purchased from more than 2 different product categories.

```sql
SELECT u.user_id, u.name, COUNT(DISTINCT c.category_id) AS category_count
FROM users u
JOIN orders o ON u.user_id = o.user_id
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id
JOIN categories c ON p.category_id = c.category_id
GROUP BY u.user_id, u.name
HAVING COUNT(DISTINCT c.category_id) > 2;
```

---

### 14. **Category Revenue Breakdown per State**

**📌 Requirement**: Show how much each state contributed to revenue in each product category.

```sql
SELECT 
    a.state, c.category_name, SUM(oi.quantity * oi.unit_price) AS revenue
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id
JOIN categories c ON p.category_id = c.category_id
JOIN users u ON o.user_id = u.user_id
JOIN addresses a ON u.user_id = a.user_id
GROUP BY a.state, c.category_name;
```

---

### 15. \*\*Find Products Ordered, Reviewed


, and Still in Cart\*\*
**📌 Requirement**: List products that users have ordered, reviewed, and also added to cart again.

```sql
SELECT DISTINCT p.name AS product_name, u.name
FROM users u
JOIN orders o ON u.user_id = o.user_id
JOIN order_items oi ON o.order_id = oi.order_id
JOIN reviews r ON r.user_id = u.user_id AND r.product_id = oi.product_id
JOIN cart c ON u.user_id = c.user_id
JOIN cart_items ci ON ci.cart_id = c.cart_id AND ci.product_id = oi.product_id
JOIN products p ON p.product_id = oi.product_id;
```

---

Excellent! Below is a set of **5 complex SQL queries**, each one designed to involve **at least 5 core SQL concepts** and **at least 5 tables** from your e-commerce schema. These queries simulate **real-world business requirements** and are built for **performance, clarity, and insight generation**.

---

## 🔥 Supercharged SQL Queries

Each query uses:

* **Joins** (inner/left)
* **Aggregations**
* **Window functions**
* **Subqueries/CTEs**
* **Case/Null handling**
* **Filtering, ordering, grouping**

---

### 🔍 **Query 1: Premium Users Who Reviewed and Reordered Top-Selling Products**

**📌 Business Requirement**:
Find users who:

1. Ordered and reviewed a product.
2. Reordered the same product later.
3. Are ranked in the top 10 by total spend.

**💡 SQL Concepts Used**:

* `JOIN`, `WINDOW FUNCTION`, `CTE`, `SUBQUERY`, `GROUP BY`, `CASE`, `ORDER BY`

```sql
WITH user_total_spend AS (
    SELECT u.user_id, u.name, SUM(o.total_amount) AS total_spent
    FROM users u
    JOIN orders o ON u.user_id = o.user_id
    GROUP BY u.user_id, u.name
),
ranked_users AS (
    SELECT *, RANK() OVER (ORDER BY total_spent DESC) AS spend_rank
    FROM user_total_spend
),
user_product_orders AS (
    SELECT o.user_id, oi.product_id, COUNT(DISTINCT o.order_id) AS order_count
    FROM orders o
    JOIN order_items oi ON o.order_id = oi.order_id
    GROUP BY o.user_id, oi.product_id
),
user_reviews AS (
    SELECT user_id, product_id
    FROM reviews
),
qualified_users AS (
    SELECT ruo.user_id, u.name, ruo.product_id, ruo.order_count
    FROM user_product_orders ruo
    JOIN user_reviews ur ON ruo.user_id = ur.user_id AND ruo.product_id = ur.product_id
    JOIN users u ON u.user_id = ruo.user_id
    WHERE ruo.order_count > 1
)
SELECT qu.user_id, qu.name, p.name AS product_name, ru.total_spent
FROM qualified_users qu
JOIN products p ON qu.product_id = p.product_id
JOIN ranked_users ru ON qu.user_id = ru.user_id
WHERE ru.spend_rank <= 10;
```

---

### 🔍 **Query 2: Revenue Breakdown by Product, Category, and User’s Location**

**📌 Business Requirement**:
Show total revenue for each product, grouped by category and user’s city & state.

**💡 SQL Concepts Used**:

* `GROUP BY`, `JOIN`, `AGGREGATE`, `COALESCE`, `ORDER BY`, `DATE FUNCTIONS`

```sql
SELECT 
    c.category_name,
    p.name AS product_name,
    a.city,
    a.state,
    COALESCE(SUM(oi.quantity * oi.unit_price), 0) AS total_revenue
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
JOIN categories c ON p.category_id = c.category_id
JOIN orders o ON oi.order_id = o.order_id
JOIN users u ON o.user_id = u.user_id
JOIN addresses a ON u.user_id = a.user_id
WHERE o.status = 'Shipped'
GROUP BY c.category_name, p.name, a.city, a.state
ORDER BY total_revenue DESC;
```

---

### 🔍 **Query 3: Top Performing Products with Reviews, Stock, and Cart Activity**

**📌 Business Requirement**:
Display products that:

* Are in stock
* Are in at least 2 carts
* Have at least 2 reviews with average rating ≥ 4
* Ranked by total order quantity

**💡 SQL Concepts Used**:

* `JOIN`, `AGGREGATE`, `GROUP BY`, `HAVING`, `RANK`, `CTE`

```sql
WITH product_reviews AS (
    SELECT product_id, COUNT(*) AS review_count, AVG(rating) AS avg_rating
    FROM reviews
    GROUP BY product_id
    HAVING COUNT(*) >= 2 AND AVG(rating) >= 4
),
product_cart_counts AS (
    SELECT product_id, COUNT(DISTINCT cart_id) AS cart_count
    FROM cart_items
    GROUP BY product_id
    HAVING COUNT(DISTINCT cart_id) >= 2
),
product_sales AS (
    SELECT product_id, SUM(quantity) AS total_sold
    FROM order_items
    GROUP BY product_id
)
SELECT 
    p.name AS product_name,
    c.category_name,
    p.stock_quantity,
    ps.total_sold,
    pr.review_count,
    pr.avg_rating,
    pc.cart_count
FROM products p
JOIN categories c ON p.category_id = c.category_id
JOIN product_reviews pr ON p.product_id = pr.product_id
JOIN product_cart_counts pc ON p.product_id = pc.product_id
JOIN product_sales ps ON p.product_id = ps.product_id
WHERE p.stock_quantity > 0
ORDER BY ps.total_sold DESC;
```

---

### 🔍 **Query 4: Fulfillment Speed Analysis by State and Category**

**📌 Business Requirement**:
Evaluate average hours between order and payment per state and product category.

**💡 SQL Concepts Used**:

* `JOIN`, `AGGREGATE`, `DATE_DIFF`, `WINDOW`, `GROUP BY`, `PARTITION BY`

```sql
SELECT 
    a.state,
    c.category_name,
    ROUND(AVG(EXTRACT(EPOCH FROM (pay.payment_date - o.order_date)) / 3600), 2) AS avg_fulfillment_hours
FROM orders o
JOIN payments pay ON o.order_id = pay.order_id
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id
JOIN categories c ON p.category_id = c.category_id
JOIN users u ON o.user_id = u.user_id
JOIN addresses a ON u.user_id = a.user_id
WHERE o.status = 'Shipped'
GROUP BY a.state, c.category_name
ORDER BY avg_fulfillment_hours;
```

---

### 🔍 **Query 5: Loyalty Tier Assignment Based on Activity**

**📌 Business Requirement**:
Classify users into loyalty tiers:

* **Platinum**: >\$2000 spent and ≥5 reviews
* **Gold**: \$1000–\$2000 and ≥2 reviews
* **Silver**: otherwise

**💡 SQL Concepts Used**:

* `JOIN`, `CASE`, `CTE`, `AGGREGATE`, `SUBQUERY`, `GROUP BY`, `WINDOW`

```sql
WITH user_spend AS (
    SELECT u.user_id, u.name, SUM(o.total_amount) AS total_spent
    FROM users u
    JOIN orders o ON u.user_id = o.user_id
    GROUP BY u.user_id, u.name
),
user_review_counts AS (
    SELECT user_id, COUNT(*) AS review_count
    FROM reviews
    GROUP BY user_id
),
tiered_users AS (
    SELECT 
        us.user_id,
        us.name,
        us.total_spent,
        COALESCE(ur.review_count, 0) AS review_count,
        CASE
            WHEN us.total_spent > 2000 AND COALESCE(ur.review_count, 0) >= 5 THEN 'Platinum'
            WHEN us.total_spent >= 1000 AND COALESCE(ur.review_count, 0) >= 2 THEN 'Gold'
            ELSE 'Silver'
        END AS loyalty_tier
    FROM user_spend us
    LEFT JOIN user_review_counts ur ON us.user_id = ur.user_id
)
SELECT * FROM tiered_users
ORDER BY total_spent DESC;
```

---

## 🧩 Summary of SQL Concepts Covered

| Query | SQL Topics                                                       |
| ----- | ---------------------------------------------------------------- |
| 1     | `JOIN`, `CTE`, `WINDOW`, `SUBQUERY`, `GROUP BY`, `FILTER`        |
| 2     | `JOIN`, `AGGREGATE`, `COALESCE`, `GROUP BY`, `ORDER BY`          |
| 3     | `CTE`, `AGGREGATE`, `HAVING`, `JOIN`, `FILTER`, `ORDER`          |
| 4     | `DATE`, `JOIN`, `GROUP BY`, `EXTRACT`, `ORDER BY`                |
| 5     | `CASE`, `CTE`, `JOIN`, `COALESCE`, `AGGREGATE`, `CLASSIFICATION` |

---

Would you like me to:

* Generate 5 more queries like these?
* Deliver this in a downloadable `.sql` script?
* Write accompanying business documentation for a project?

To help you build SQL queries based on business requirements, it’s critical to first **break down the requirements into components** that directly translate to database elements. Here’s a structured approach to **Business Requirement Breakdown Analysis for SQL Query Building**:

---

### 🔍 1. Understand the Business Requirement

Ask:

* **What is the business goal?**

  * Example: "Get total sales for each product by region for Q1 2025."
* **Who is the end user?**

  * Example: Sales Manager? Data Analyst?
* **How often is this needed?**

  * One-time report, dashboard, daily batch?

---

### 📊 2. Identify Key Entities and Metrics

Ask:

* **What are the key entities (nouns)?**

  * E.g., `sales`, `products`, `regions`, `dates`
* **What are the metrics (quantitative data)?**

  * E.g., `total_sales`, `quantity_sold`, `revenue`
* **What are the dimensions (categorical groupings)?**

  * E.g., `product_name`, `region_name`, `quarter`, `customer_segment`

---

### 🗂️ 3. Map to Database Tables

* Identify which **tables** hold each entity or metric.

  * `sales_fact`, `products_dim`, `regions_dim`, `date_dim`
* Confirm **primary keys** and **foreign keys** for joins.
* Validate data types and field names.

---

### ⚙️ 4. Define Filters and Conditions

Ask:

* **Time filters?**

  * E.g., `sale_date BETWEEN '2025-01-01' AND '2025-03-31'`
* **Status filters?**

  * E.g., Only completed transactions: `status = 'Completed'`
* **Customer filters?**

  * Specific segment or region?

---

### 🧮 5. Define Aggregations or Calculations

* What needs to be **aggregated**?

  * `SUM(sales_amount)`
  * `COUNT(order_id)`
* Any **derived metrics**?

  * Profit margin = `(sales_amount - cost) / sales_amount`

---

### 🧱 6. Determine Output Format

* What columns are expected?

  * `product_name`, `region_name`, `total_sales`
* How should results be grouped?

  * `GROUP BY product_name, region_name`
* Any ordering or limits?

  * `ORDER BY total_sales DESC`
  * `LIMIT 10`

---

### ✅ 7. Verify Assumptions

* Validate field names with data dictionary or ERD.
* Check sample data if possible.
* Get clarification if the requirement is ambiguous.

---

### Example Breakdown:

**Business Requirement**:
“Show the top 5 products by revenue in North America for Q1 2025.”

#### Breakdown:

| Aspect     | Detail                                         |
| ---------- | ---------------------------------------------- |
| Goal       | Identify top-selling products                  |
| Metrics    | Total revenue (`SUM(sales_amount)`)            |
| Dimensions | Product name, region                           |
| Tables     | `sales`, `products`, `regions`, `date_dim`     |
| Filters    | Region = 'North America', Sale date in Q1 2025 |
| Grouping   | By product                                     |
| Ordering   | By revenue DESC                                |
| Limiting   | Top 5                                          |

#### Resulting SQL (simplified):

```sql
SELECT 
    p.product_name,
    SUM(s.sales_amount) AS total_revenue
FROM 
    sales s
JOIN 
    products p ON s.product_id = p.product_id
JOIN 
    regions r ON s.region_id = r.region_id
JOIN 
    date_dim d ON s.sale_date = d.date
WHERE 
    r.region_name = 'North America'
    AND d.quarter = 'Q1'
    AND d.year = 2025
GROUP BY 
    p.product_name
ORDER BY 
    total_revenue DESC
LIMIT 5;
```

---
Absolutely! Below are **5 business requirement breakdowns**, each with a clear analysis and example SQL approach.

---

### **1. Requirement:**

**“Find the number of new customers acquired each month in 2024.”**

#### 🔍 Breakdown:

| Element        | Description                          |
| -------------- | ------------------------------------ |
| **Goal**       | Track customer acquisition over time |
| **Metric**     | `COUNT(customer_id)`                 |
| **Entity**     | `customers` table                    |
| **Date field** | `created_at` or `registration_date`  |
| **Filter**     | Year = 2024                          |
| **Group by**   | Month                                |
| **Output**     | `month`, `customer_count`            |

#### 💻 SQL:

```sql
SELECT 
    DATE_TRUNC('month', created_at) AS month,
    COUNT(customer_id) AS new_customers
FROM 
    customers
WHERE 
    created_at BETWEEN '2024-01-01' AND '2024-12-31'
GROUP BY 
    DATE_TRUNC('month', created_at)
ORDER BY 
    month;
```

---

### **2. Requirement:**

**“Get average order value by customer segment in the last 6 months.”**

#### 🔍 Breakdown:

| Element        | Description                    |
| -------------- | ------------------------------ |
| **Goal**       | Measure customer segment value |
| **Metric**     | `AVG(order_amount)`            |
| **Entities**   | `orders`, `customers`          |
| **Date field** | `order_date`                   |
| **Filter**     | Order date = last 6 months     |
| **Group by**   | Customer segment               |

#### 💻 SQL:

```sql
SELECT 
    c.segment,
    AVG(o.total_amount) AS avg_order_value
FROM 
    orders o
JOIN 
    customers c ON o.customer_id = c.customer_id
WHERE 
    o.order_date >= CURRENT_DATE - INTERVAL '6 months'
GROUP BY 
    c.segment;
```

---

### **3. Requirement:**

**“List all products with zero sales in Q2 2025.”**

#### 🔍 Breakdown:

| Element             | Description                       |
| ------------------- | --------------------------------- |
| **Goal**            | Identify underperforming products |
| **Entities**        | `products`, `sales`               |
| **Sales condition** | Product not found in sales table  |
| **Date filter**     | Q2 2025                           |

#### 💻 SQL:

```sql
SELECT 
    p.product_id,
    p.product_name
FROM 
    products p
LEFT JOIN 
    sales s ON p.product_id = s.product_id 
           AND s.sale_date BETWEEN '2025-04-01' AND '2025-06-30'
WHERE 
    s.sale_id IS NULL;
```

---

### **4. Requirement:**

**“Show top 3 regions with highest profit margin this year.”**

#### 🔍 Breakdown:

| Element         | Description                  |
| --------------- | ---------------------------- |
| **Metric**      | `(revenue - cost) / revenue` |
| **Entities**    | `sales`, `regions`           |
| **Date filter** | Current year                 |
| **Group by**    | Region                       |
| **Order by**    | Profit margin DESC           |
| **Limit**       | 3                            |

#### 💻 SQL:

```sql
SELECT 
    r.region_name,
    SUM(s.revenue - s.cost) / NULLIF(SUM(s.revenue), 0) AS profit_margin
FROM 
    sales s
JOIN 
    regions r ON s.region_id = r.region_id
WHERE 
    EXTRACT(YEAR FROM s.sale_date) = EXTRACT(YEAR FROM CURRENT_DATE)
GROUP BY 
    r.region_name
ORDER BY 
    profit_margin DESC
LIMIT 3;
```

---

### **5. Requirement:**

**“Calculate the churn rate for subscription customers in Q1 2025.”**

#### 🔍 Breakdown:

| Element                   | Description                                 |
| ------------------------- | ------------------------------------------- |
| **Metric**                | Churn Rate = (Lost customers / Start count) |
| **Entities**              | `subscriptions`, `customers`                |
| **Active users Q1 start** | Active on Jan 1                             |
| **Churned**               | Cancelled before Apr 1                      |

#### 💻 SQL:

```sql
WITH q1_start AS (
    SELECT customer_id
    FROM subscriptions
    WHERE start_date <= '2025-01-01'
      AND (end_date IS NULL OR end_date >= '2025-01-01')
),
q1_end AS (
    SELECT customer_id
    FROM subscriptions
    WHERE end_date BETWEEN '2025-01-01' AND '2025-03-31'
)
SELECT 
    COUNT(DISTINCT q1_end.customer_id) * 1.0 / COUNT(DISTINCT q1_start.customer_id) AS churn_rate
FROM 
    q1_start
LEFT JOIN 
    q1_end ON q1_start.customer_id = q1_end.customer_id;
```

---

Great — let's tailor 3 business requirement breakdowns specifically for **Netflix**-type data. We'll focus on typical streaming service analytics use cases involving **users**, **subscriptions**, **viewing data**, **content**, and **engagement**.

---

### ✅ **1. Requirement:**

**“Find the top 5 most-watched TV shows globally in Q2 2025.”**

#### 🔍 Breakdown:

| Element          | Description                              |
| ---------------- | ---------------------------------------- |
| **Goal**         | Identify globally top-performing content |
| **Metric**       | `SUM(watch_time)` or `COUNT(view_id)`    |
| **Entities**     | `views`, `titles`                        |
| **Content type** | Only TV shows                            |
| **Date filter**  | Q2 2025                                  |
| **Group by**     | `title_name`                             |
| **Order by**     | Descending watch time or view count      |
| **Limit**        | 5                                        |

#### 💻 SQL:

```sql
SELECT 
    t.title_name,
    SUM(v.watch_time_minutes) AS total_watch_time
FROM 
    views v
JOIN 
    titles t ON v.title_id = t.title_id
WHERE 
    t.content_type = 'TV Show'
    AND v.view_date BETWEEN '2025-04-01' AND '2025-06-30'
GROUP BY 
    t.title_name
ORDER BY 
    total_watch_time DESC
LIMIT 5;
```

---

### ✅ **2. Requirement:**

**“Calculate churn rate in the US for Standard and Premium plans in Q1 2025.”**

#### 🔍 Breakdown:

| Element        | Description                                    |
| -------------- | ---------------------------------------------- |
| **Goal**       | Measure customer retention                     |
| **Metric**     | `(churned / starting_subscriptions)`           |
| **Entities**   | `subscriptions`, `users`                       |
| **Filters**    | Country = 'US', Plan = 'Standard' or 'Premium' |
| **Time range** | Jan–Mar 2025                                   |
| **Group by**   | Plan                                           |

#### 💻 SQL:

```sql
WITH starting_subs AS (
    SELECT user_id, plan_type
    FROM subscriptions
    WHERE start_date <= '2025-01-01'
      AND (end_date IS NULL OR end_date >= '2025-01-01')
      AND country = 'US'
      AND plan_type IN ('Standard', 'Premium')
),
churned_subs AS (
    SELECT user_id, plan_type
    FROM subscriptions
    WHERE end_date BETWEEN '2025-01-01' AND '2025-03-31'
      AND country = 'US'
      AND plan_type IN ('Standard', 'Premium')
)
SELECT 
    s.plan_type,
    COUNT(DISTINCT c.user_id) * 1.0 / COUNT(DISTINCT s.user_id) AS churn_rate
FROM 
    starting_subs s
LEFT JOIN 
    churned_subs c ON s.user_id = c.user_id AND s.plan_type = c.plan_type
GROUP BY 
    s.plan_type;
```

---

### ✅ **3. Requirement:**

**“Determine average daily watch time per user in India during June 2025.”**

#### 🔍 Breakdown:

| Element               | Description                                      |
| --------------------- | ------------------------------------------------ |
| **Goal**              | Understand engagement by geography               |
| **Metric**            | `SUM(watch_time) / COUNT(DISTINCT days * users)` |
| **Entities**          | `views`, `users`                                 |
| **Date filter**       | June 2025                                        |
| **Geographic filter** | `country = 'India'`                              |
| **Group by**          | None (overall metric)                            |

#### 💻 SQL:

```sql
SELECT 
    SUM(v.watch_time_minutes) * 1.0 / 
    (COUNT(DISTINCT v.view_date) * COUNT(DISTINCT v.user_id)) AS avg_daily_watch_time_per_user
FROM 
    views v
JOIN 
    users u ON v.user_id = u.user_id
WHERE 
    u.country = 'India'
    AND v.view_date BETWEEN '2025-06-01' AND '2025-06-30';
```

---

Sure! Here are 10 common MySQL DBA-related operations along with their sample queries:

---

### 1. **Create a new database**

```sql
CREATE DATABASE my_database;
```

---

### 2. **Create a new user and grant privileges**

```sql
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password123';
GRANT ALL PRIVILEGES ON my_database.* TO 'newuser'@'localhost';
FLUSH PRIVILEGES;
```

---

### 3. **Backup a database (using `mysqldump` from command line)**

```bash
mysqldump -u root -p my_database > my_database_backup.sql
```

---

### 4. **Restore a database from a backup**

```bash
mysql -u root -p my_database < my_database_backup.sql
```

---

### 5. **Show current running processes**

```sql
SHOW PROCESSLIST;
```

---

### 6. **Check database size**

```sql
SELECT table_schema AS database_name,
       ROUND(SUM(data_length + index_length) / 1024 / 1024, 2) AS size_mb
FROM information_schema.tables
GROUP BY table_schema;
```

---

### 7. **Monitor MySQL server status**

```sql
SHOW STATUS LIKE '%Threads%';
```

---

### 8. **Add a new column to an existing table**

```sql
ALTER TABLE employees ADD COLUMN department VARCHAR(50);
```

---

### 9. **Check the MySQL error log location**

```sql
SHOW VARIABLES LIKE 'log_error';
```

---

### 10. **Optimize a table**

```sql
OPTIMIZE TABLE employees;
```

---

Got it! Here are **15 more complex MySQL DBA tasks** with corresponding queries:

---

### 1. **Identify slow queries using the slow query log**

```sql
SHOW VARIABLES LIKE 'slow_query_log';
-- Enable slow query log if disabled:
SET GLOBAL slow_query_log = 'ON';
SET GLOBAL slow_query_log_file = '/var/log/mysql/mysql-slow.log';
```

---

### 2. **Analyze slow queries using `EXPLAIN`**

```sql
EXPLAIN SELECT * FROM orders WHERE customer_id = 1234;
```

---

### 3. **Create a partitioned table**

```sql
CREATE TABLE sales (
    sale_id INT NOT NULL,
    sale_date DATE NOT NULL,
    amount DECIMAL(10,2),
    PRIMARY KEY (sale_id, sale_date)
)
PARTITION BY RANGE (YEAR(sale_date)) (
    PARTITION p2019 VALUES LESS THAN (2020),
    PARTITION p2020 VALUES LESS THAN (2021),
    PARTITION pmax VALUES LESS THAN MAXVALUE
);
```

---

### 4. **Perform point-in-time recovery using binary logs**

* Enable binary logging:

```sql
SHOW VARIABLES LIKE 'log_bin';
-- If not enabled, add in my.cnf: log-bin=mysql-bin
```

* To apply logs to restore:

```bash
mysqlbinlog mysql-bin.000001 | mysql -u root -p my_database
```

---

### 5. **Monitor replication status**

```sql
SHOW SLAVE STATUS\G
```

---

### 6. **Stop and start replication slave**

```sql
STOP SLAVE;
START SLAVE;
```

---

### 7. **Add an index for query optimization**

```sql
CREATE INDEX idx_customer_id ON orders (customer_id);
```

---

### 8. **Remove unused indexes**

```sql
DROP INDEX idx_customer_id ON orders;
```

---

### 9. **Change a user’s password**

```sql
ALTER USER 'newuser'@'localhost' IDENTIFIED BY 'newpassword456';
FLUSH PRIVILEGES;
```

---

### 10. **Export a specific table to CSV**

```sql
SELECT * FROM employees
INTO OUTFILE '/tmp/employees.csv'
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n';
```

---

### 11. **Create a stored procedure**

```sql
DELIMITER //
CREATE PROCEDURE GetEmployeeCount()
BEGIN
    SELECT COUNT(*) FROM employees;
END //
DELIMITER ;
```

---

### 12. **View all stored procedures**

```sql
SHOW PROCEDURE STATUS WHERE Db = 'my_database';
```

---

### 13. **Enable general query log for debugging**

```sql
SET GLOBAL general_log = 'ON';
SET GLOBAL general_log_file = '/var/log/mysql/mysql-general.log';
```

---

### 14. **Check table fragmentation and free space**

```sql
SELECT table_name,
       data_free
FROM information_schema.tables
WHERE table_schema = 'my_database';
```

---

### 15. **Migrate a table to InnoDB from MyISAM**

```sql
ALTER TABLE my_table ENGINE = InnoDB;
```

---

Absolutely! Let's dive deep into **`EXPLAIN PLAN`** in SQL — what it is, how to use it, and how it helps improve query performance in various ways.

---

## What is EXPLAIN PLAN?

`EXPLAIN PLAN` is a SQL command used to display the execution plan that the database optimizer will use for a given query. The execution plan shows the steps and methods the database engine will follow to retrieve the requested data. It helps you understand **how** the database will execute your SQL query, which is crucial for performance tuning.

---

## Why use EXPLAIN PLAN?

* To **analyze** the efficiency of your SQL queries.
* To **identify bottlenecks** or inefficient operations.
* To **understand index usage** and joins.
* To help **optimize queries** and improve overall database performance.

---

## How EXPLAIN PLAN works?

1. You write an SQL query.
2. Run `EXPLAIN PLAN FOR <your query>`.
3. Query the plan table (or use GUI tools) to see the steps the database will take.
4. Analyze the plan to see if any operations are costly (full table scans, nested loops, etc.).
5. Use findings to optimize query or schema.

---

## Example (Oracle syntax)

```sql
EXPLAIN PLAN FOR
SELECT * FROM employees WHERE department_id = 10;

SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY);
```

---

# 10 Ways EXPLAIN PLAN Helps Improve Performance

### 1. **Identify Full Table Scans**

* Full table scans mean the database reads every row, which is slow on large tables.
* EXPLAIN PLAN shows if your query does this.
* Solution: Add indexes or rewrite the query to avoid full scans.

---

### 2. **Detect Missing or Unused Indexes**

* The plan reveals whether indexes are used.
* If no index is used on WHERE conditions, consider creating them.
* Also, detect unused indexes that can be dropped.

---

### 3. **Analyze Join Methods**

* Joins can be nested loops, hash joins, or merge joins.
* EXPLAIN PLAN shows which join type is used.
* For large tables, hash or merge joins are faster than nested loops.
* Tune queries or add hints based on join strategy.

---

### 4. **Understand Filter and Access Paths**

* Shows how filters are applied.
* Helps optimize WHERE clauses and predicates for better access paths.
* e.g., use SARGable predicates to enable index use.

---

### 5. **Check Estimated Rows and Costs**

* EXPLAIN PLAN shows estimated number of rows processed at each step.
* High estimated rows could indicate bad statistics or inefficient query.
* Update statistics or rewrite query accordingly.

---

### 6. **Pinpoint Expensive Operations**

* Operations with high cost or long time estimates are candidates for optimization.
* Could be sorts, aggregations, or functions.
* Can be optimized by rewriting query or indexing.

---

### 7. **Detect Cartesian Products (Cross Joins)**

* Cartesian products (joins without join condition) lead to huge result sets.
* EXPLAIN PLAN shows if they happen.
* Fix join conditions to avoid unnecessary data explosion.

---

### 8. **Optimize Subqueries and Nested Queries**

* EXPLAIN PLAN reveals if subqueries are converted to joins or executed repeatedly.
* Helps decide if subqueries should be rewritten or use WITH clauses (CTEs).

---

### 9. **Evaluate Parallel Execution**

* Shows if parallel query execution is planned.
* Parallelism can improve performance on big data.
* You can tune degree of parallelism accordingly.

---

### 10. **Confirm Effects of Query Hints**

* You can add hints (e.g., `/*+ INDEX(emp emp_dept_idx) */`) to influence optimizer.
* EXPLAIN PLAN shows if hint took effect.
* Helps fine-tune performance by guiding optimizer.

---

# Summary

| Aspect                        | What EXPLAIN PLAN Shows          | How It Helps Improve Performance       |
| ----------------------------- | -------------------------------- | -------------------------------------- |
| Full Table Scans              | Detects full scans               | Add indexes, rewrite query             |
| Index Usage                   | Whether indexes are used         | Create/drop indexes as needed          |
| Join Types                    | Nested loops, hash, merge join   | Change query or hints to optimize join |
| Filter Usage                  | Predicate pushdown, access paths | Use SARGable predicates                |
| Estimated Rows and Cost       | Number of rows & cost per step   | Tune query/statistics                  |
| Expensive Operations          | Sorts, aggregates, functions     | Rewrite queries, add indexing          |
| Cartesian Products            | Cross join detection             | Add join conditions                    |
| Subqueries and Nested Queries | Execution method                 | Rewrite or use CTE                     |
| Parallel Execution            | Parallelism planned or not       | Adjust parallel degree                 |
| Query Hints                   | Whether hints are obeyed         | Guide optimizer for better plan        |

---

Sure! Let’s go step-by-step with **EXPLAIN** in MySQL, giving you example queries, their EXPLAIN output, and detailed analysis showing how to improve performance.

---

# 1. Simple WHERE with no index — Full table scan

**Query:**

```sql
SELECT * FROM employees WHERE department_id = 10;
```

**Assuming no index on `department_id`.**

---

**EXPLAIN output:**

| id | select\_type | table     | type | possible\_keys | key  | key\_len | ref  | rows | Extra       |
| -- | ------------ | --------- | ---- | -------------- | ---- | -------- | ---- | ---- | ----------- |
| 1  | SIMPLE       | employees | ALL  | NULL           | NULL | NULL     | NULL | 1000 | Using where |

---

**Analysis:**

* `type = ALL`: Full table scan — reads all rows.
* `possible_keys = NULL` means no indexes are usable.
* `rows = 1000` means estimated scanning 1000 rows.
* `Extra = Using where` means filtering after scan.
* Slow on large tables — full scan every time.

**How to improve:**

* Add an index on `department_id` to speed up filtering.
* Example: `CREATE INDEX idx_dept_id ON employees(department_id);`

---

# 2. Same query after adding index on `department_id`

**Query:**

```sql
SELECT * FROM employees WHERE department_id = 10;
```

---

**EXPLAIN output:**

| id | select\_type | table     | type | possible\_keys | key           | key\_len | ref   | rows | Extra       |
| -- | ------------ | --------- | ---- | -------------- | ------------- | -------- | ----- | ---- | ----------- |
| 1  | SIMPLE       | employees | ref  | idx\_dept\_id  | idx\_dept\_id | 4        | const | 50   | Using index |

---

**Analysis:**

* `type = ref`: uses index to find matching rows.
* `key = idx_dept_id`: index is used.
* `rows = 50`: scans estimated 50 rows instead of all 1000.
* `Extra = Using index`: index-only scan, no need to read full row.
* Much faster and efficient.

---

# 3. Join without indexes — Nested loop full scan

**Query:**

```sql
SELECT e.name, d.department_name
FROM employees e
JOIN departments d ON e.department_id = d.id
WHERE d.location = 'NY';
```

**Assuming no index on `departments.location` or `employees.department_id`.**

---

**EXPLAIN output:**

| id | select\_type | table       | type | possible\_keys | key  | key\_len | ref  | rows | Extra       |
| -- | ------------ | ----------- | ---- | -------------- | ---- | -------- | ---- | ---- | ----------- |
| 1  | SIMPLE       | departments | ALL  | NULL           | NULL | NULL     | NULL | 50   | Using where |
| 1  | SIMPLE       | employees   | ALL  | NULL           | NULL | NULL     | NULL | 1000 | Using where |

---

**Analysis:**

* Both tables use `type = ALL` full table scans — very slow.
* No indexes available for join or filtering.
* Rows scanned multiply quickly — slow for big data.
* `Using where` means filtering after scan.
* Bad join strategy with high cost.

**Improvement:**

* Add index on `departments.location`.
* Add index on `employees.department_id`.

---

# 4. After adding indexes on join and filter columns

**EXPLAIN output:**

| id | select\_type | table       | type | possible\_keys      | key                 | key\_len | ref   | rows | Extra       |
| -- | ------------ | ----------- | ---- | ------------------- | ------------------- | -------- | ----- | ---- | ----------- |
| 1  | SIMPLE       | departments | ref  | idx\_location       | idx\_location       | 42       | const | 10   | Using index |
| 1  | SIMPLE       | employees   | ref  | idx\_department\_id | idx\_department\_id | 4        | d.id  | 20   | Using index |

---

**Analysis:**

* `type = ref` on both tables — efficient index lookup.
* `key` columns show indexes used.
* Rows scanned dropped significantly (10 and 20).
* `Using index` means index-only scan, fast.
* Join performed via indexed columns — optimized.

---

# 5. Query with ORDER BY without index — filesort

**Query:**

```sql
SELECT * FROM employees ORDER BY hire_date DESC LIMIT 10;
```

---

**EXPLAIN output:**

| id | select\_type | table     | type | possible\_keys | key  | key\_len | ref  | rows | Extra          |
| -- | ------------ | --------- | ---- | -------------- | ---- | -------- | ---- | ---- | -------------- |
| 1  | SIMPLE       | employees | ALL  | NULL           | NULL | NULL     | NULL | 1000 | Using filesort |

---

**Analysis:**

* `type = ALL` full scan.
* No index to help with ORDER BY.
* `Extra = Using filesort` means MySQL sorts results after fetching.
* Sorting large data inefficient and slow.
* Limit helps, but still costly.

---

# 6. Adding index on `hire_date` to optimize ORDER BY

**EXPLAIN output:**

| id | select\_type | table     | type  | possible\_keys  | key             | key\_len | ref  | rows | Extra       |
| -- | ------------ | --------- | ----- | --------------- | --------------- | -------- | ---- | ---- | ----------- |
| 1  | SIMPLE       | employees | index | idx\_hire\_date | idx\_hire\_date | 4        | NULL | 10   | Using index |

---

**Analysis:**

* `type = index`: reads data from index only (which is sorted).
* Uses `idx_hire_date` index for sorting — no filesort.
* Rows scanned reduced to 10 (due to LIMIT).
* `Using index` means index-only scan.
* Fast query with minimal cost.

---

# 7. Subquery without optimization — dependent subquery

**Query:**

```sql
SELECT name FROM employees WHERE department_id IN (SELECT id FROM departments WHERE location = 'NY');
```

---

**EXPLAIN output:**

| id | select\_type       | table       | type | possible\_keys | key  | key\_len | ref  | rows | Extra                   |
| -- | ------------------ | ----------- | ---- | -------------- | ---- | -------- | ---- | ---- | ----------------------- |
| 1  | PRIMARY            | employees   | ALL  | NULL           | NULL | NULL     | NULL | 1000 | Using where             |
| 2  | DEPENDENT SUBQUERY | departments | ALL  | NULL           | NULL | NULL     | NULL | 50   | Using where; correlated |

---

**Analysis:**

* Outer query does full scan (type ALL).
* Subquery is correlated and full scan on departments.
* This results in a nested loop and poor performance.
* Rows scanned multiply (1000 \* 50).
* Very inefficient on large data.

---

# 8. Rewriting subquery as join to optimize

**Query:**

```sql
SELECT e.name FROM employees e JOIN departments d ON e.department_id = d.id WHERE d.location = 'NY';
```

---

**EXPLAIN output:**

| id | select\_type | table       | type | possible\_keys      | key                 | key\_len | ref   | rows | Extra       |
| -- | ------------ | ----------- | ---- | ------------------- | ------------------- | -------- | ----- | ---- | ----------- |
| 1  | SIMPLE       | departments | ref  | idx\_location       | idx\_location       | 42       | const | 10   | Using index |
| 1  | SIMPLE       | employees   | ref  | idx\_department\_id | idx\_department\_id | 4        | d.id  | 20   | Using index |

---

**Analysis:**

* Join uses indexes efficiently.
* Rows scanned drastically reduced.
* No dependent subquery.
* Faster execution, better optimization.
* Always prefer join over dependent subqueries in MySQL.

---

# 9. GROUP BY without index — Using temporary + filesort

**Query:**

```sql
SELECT department_id, COUNT(*) FROM employees GROUP BY department_id;
```

---

**EXPLAIN output:**

| id | select\_type | table     | type | possible\_keys | key  | key\_len | ref  | rows | Extra                           |
| -- | ------------ | --------- | ---- | -------------- | ---- | -------- | ---- | ---- | ------------------------------- |
| 1  | SIMPLE       | employees | ALL  | NULL           | NULL | NULL     | NULL | 1000 | Using temporary; Using filesort |

---

**Analysis:**

* Full scan with no index.
* `Using temporary; Using filesort` means MySQL creates a temp table and sorts.
* This is slow on large datasets.
* Could become bottleneck.

---

# 10. Adding index on `department_id` to optimize GROUP BY

**EXPLAIN output:**

| id | select\_type | table     | type  | possible\_keys      | key                 | key\_len | ref  | rows | Extra       |
| -- | ------------ | --------- | ----- | ------------------- | ------------------- | -------- | ---- | ---- | ----------- |
| 1  | SIMPLE       | employees | index | idx\_department\_id | idx\_department\_id | 4        | NULL | 1000 | Using index |

---

**Analysis:**

* `type = index`: scans index in sorted order.
* No temporary table or filesort needed.
* Index covers GROUP BY column, speeding aggregation.
* Efficient aggregation on large data.
* Index design crucial for GROUP BY performance.

---

# Summary

| Use Case                     | EXPLAIN Clue                      | Performance Tip               |
| ---------------------------- | --------------------------------- | ----------------------------- |
| Full table scan              | type = ALL                        | Add indexes                   |
| Index usage                  | key not null                      | Use correct indexes           |
| Join with no index           | type = ALL on joined table        | Add join keys indexes         |
| Filesort in ORDER BY         | Extra = Using filesort            | Add index on ORDER BY columns |
| Temporary tables in GROUP BY | Extra = Using temporary           | Add index on GROUP BY columns |
| Dependent subquery           | select\_type = DEPENDENT SUBQUERY | Rewrite as JOIN               |
| Index-only scan              | Extra = Using index               | Design covering indexes       |

---

Great choice! Let’s start with **SQL Transactions** in a detailed, practical way — following the same structure as before: explanation, use cases, example queries, performance insights, and analysis.

---

# ✅ SQL Topic: **Transactions**

---

## 🔍 What is a Transaction?

A **transaction** in SQL is a sequence of one or more SQL statements that are executed **as a single unit of work**. A transaction ensures **data integrity** through the **ACID** properties:

### ACID Properties:

* **Atomicity**: All or nothing (either all steps succeed, or none do)
* **Consistency**: The database moves from one valid state to another
* **Isolation**: Transactions don’t interfere with each other
* **Durability**: Once committed, changes persist even after crashes

---

## 🎯 Use Cases for Transactions

* Transferring money between accounts
* Inserting related records in multiple tables
* Rolling back changes if a validation fails
* Batch updates that must complete together
* Complex business logic enforcement

---

## ⚙️ Transaction Control Statements in SQL

| Statement                     | Description                                   |
| ----------------------------- | --------------------------------------------- |
| `START TRANSACTION` / `BEGIN` | Begins a new transaction                      |
| `COMMIT`                      | Saves all changes made in the transaction     |
| `ROLLBACK`                    | Undoes all changes made in the transaction    |
| `SAVEPOINT`                   | Sets a point in a transaction to roll back to |
| `RELEASE SAVEPOINT`           | Deletes a savepoint                           |
| `SET TRANSACTION`             | Sets isolation level (e.g., READ COMMITTED)   |

---

## 🧪 Example 1: Simple Bank Transfer (WITH EXPLAIN ANALYSIS)

### 🔸 Query

```sql
START TRANSACTION;

UPDATE accounts SET balance = balance - 500 WHERE account_id = 101;
UPDATE accounts SET balance = balance + 500 WHERE account_id = 202;

COMMIT;
```

---

### 🔍 EXPLAIN for one UPDATE:

```sql
EXPLAIN UPDATE accounts SET balance = balance - 500 WHERE account_id = 101;
```

#### 📋 EXPLAIN Output

| id | select\_type | table    | type  | possible\_keys | key     | key\_len | ref   | rows | Extra       |
| -- | ------------ | -------- | ----- | -------------- | ------- | -------- | ----- | ---- | ----------- |
| 1  | SIMPLE       | accounts | const | PRIMARY        | PRIMARY | 4        | const | 1    | Using where |

---

### 📌 Analysis

* `type = const`: Updating by primary key — very efficient.
* `key = PRIMARY`: Index used — no full scan.
* `rows = 1`: Minimal data access.
* Transaction groups multiple updates into a unit.
* Fast and safe — ensures atomic transfer.

---

## 🧪 Example 2: Rollback on Error

### 🔸 Query

```sql
START TRANSACTION;

INSERT INTO orders (order_id, customer_id, total_amount)
VALUES (1005, 201, 250);

INSERT INTO payments (payment_id, order_id, amount)
VALUES (5005, 9999, 250); -- 9999 does not exist in orders

ROLLBACK;
```

---

### 🔍 EXPLAIN for INSERT:

```sql
EXPLAIN INSERT INTO payments (payment_id, order_id, amount)
VALUES (5005, 9999, 250);
```

#### 📋 EXPLAIN Output

| id  | select\_type | table    | type | possible\_keys | key  | key\_len | ref  | rows | Extra    |
| --- | ------------ | -------- | ---- | -------------- | ---- | -------- | ---- | ---- | -------- |
| N/A | INSERT       | payments | N/A  | order\_id\_fk  | NULL | NULL     | NULL | NULL | FK check |

---

### 📌 Analysis

* The INSERT fails due to **foreign key violation**.
* Transaction **rolls back all changes** — no partial inserts.
* Maintains **referential integrity**.
* No partial state is saved — critical in financial applications.
* `EXPLAIN` on INSERT is limited but useful for understanding FK checks.

---

## 🔐 Isolation Levels (SET TRANSACTION)

### Levels:

| Level            | Description                                            |
| ---------------- | ------------------------------------------------------ |
| READ UNCOMMITTED | Dirty reads allowed — fastest, least safe              |
| READ COMMITTED   | Only committed changes visible (default in many RDBMS) |
| REPEATABLE READ  | Same rows return same data in a transaction            |
| SERIALIZABLE     | Full isolation — transactions run one after another    |

---

### 🔸 Example: Setting Isolation Level

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;

START TRANSACTION;
-- query logic here
COMMIT;
```

---

## 🧠 10 Ways Transactions Help Improve Performance or Reliability

| #  | Scenario                                    | Benefit                                  |
| -- | ------------------------------------------- | ---------------------------------------- |
| 1  | Batch inserts with rollback on failure      | Prevents partial data                    |
| 2  | Financial operations (transfer, billing)    | Ensures atomic money flow                |
| 3  | Inventory update with order processing      | Avoids over-selling                      |
| 4  | Nested savepoints for partial undo          | Granular rollback control                |
| 5  | Complex business rules                      | Commit only on full rule validation      |
| 6  | Error handling and retries                  | Simplifies error recovery                |
| 7  | Locks with isolation                        | Prevents dirty reads and race conditions |
| 8  | Reduces need for manual consistency checks  | Guarantees consistency                   |
| 9  | Improves concurrency with correct isolation | Parallel safe processing                 |
| 10 | Debugging & testing logic                   | Reversible operations aid testing        |

---

## ✅ Best Practices for Transactions

* Keep transactions **short and fast**.
* Always **commit or rollback explicitly**.
* Avoid user interaction **inside a transaction**.
* Use appropriate **isolation levels** for your use case.
* **Index relevant keys** to speed up transactional updates.

---

Here is a **sample MySQL Slow Query Log output**, followed by an analysis with **at least 5 performance insights**:

---

### 📄 **Sample Slow Query Log Output**

```
# Time: 2025-07-13T10:45:12.567890Z
# User@Host: app_user[app_user] @ localhost []
# Thread_id: 1572  Schema: ecommerce  QC_hit: No
# Query_time: 12.354654  Lock_time: 0.000298  Rows_sent: 12500  Rows_examined: 150000
SET timestamp=1752345912;
SELECT * FROM orders WHERE status = 'pending' ORDER BY created_at DESC;
```

---

### 🔍 **Analysis & Optimization Insights**

1. **⚠️ Query Time is High**

   * `Query_time: 12.35` seconds indicates this query is significantly slow and likely affecting user experience.
   * Goal: Bring this below 1 second for a better-performing system.

2. **🔍 Too Many Rows Examined**

   * `Rows_examined: 150000` vs. `Rows_sent: 12500` suggests inefficient filtering.
   * Action: Ensure the `status` column is indexed to reduce rows scanned.

   ```sql
   CREATE INDEX idx_orders_status ON orders(status);
   ```

3. **📦 Potential Over-fetching (`SELECT *`)**

   * Fetching all columns with `SELECT *` may be unnecessary.
   * Action: Specify only needed columns to reduce I/O and data transfer.

   ```sql
   SELECT id, user_id, total_amount, created_at FROM orders WHERE status = 'pending';
   ```

4. **📉 No Query Cache Hit**

   * `QC_hit: No` shows the query wasn't served from cache.
   * If using MySQL 5.7 or earlier, consider tuning the query cache or results caching in your app layer.

5. **📊 ORDER BY on Large Result Set**

   * Sorting `ORDER BY created_at DESC` without a proper index can cause full table scans.
   * Action: Add a composite index if applicable.

   ```sql
   CREATE INDEX idx_status_created ON orders(status, created_at DESC);
   ```

---

Would you like me to simulate more log entries or analyze a real one from your system (if you can upload or paste it)?



