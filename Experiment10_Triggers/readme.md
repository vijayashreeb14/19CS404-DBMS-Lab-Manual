# Experiment 10: PL/SQL – Triggers

## AIM
To write and execute PL/SQL trigger programs for automating actions in response to specific table events like INSERT, UPDATE, or DELETE.

---

## THEORY

A **trigger** is a stored PL/SQL block that is automatically executed or fired when a specified event occurs on a table or view. Triggers can be used for enforcing business rules, auditing changes, or automatic updates.

### Types of Triggers:
- **Before Trigger**: Executes before the operation (INSERT, UPDATE, DELETE).
- **After Trigger**: Executes after the operation.
- **Row-level Trigger**: Executes for each affected row.
- **Statement-level Trigger**: Executes once for the triggering statement.

**Basic Syntax:**
```sql
CREATE OR REPLACE TRIGGER trigger_name
BEFORE|AFTER INSERT|UPDATE|DELETE ON table_name
[FOR EACH ROW]
BEGIN
   -- trigger logic
END;
```

## 1. Write a trigger to log every insertion into a table.
**Steps:**
- Create two tables: `employees` (for storing data) and `employee_log` (for logging the inserts).
- Write an **AFTER INSERT** trigger on the `employees` table to log the new data into the `employee_log` table.
## Code:
```
CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50),
    salary NUMBER,
    dept_no NUMBER,
    designation VARCHAR2(50)
);
CREATE TABLE employee_log (
    log_id NUMBER GENERATED ALWAYS AS IDENTITY,
    emp_id NUMBER,
    emp_name VARCHAR2(50),
    salary NUMBER,
    dept_no NUMBER,
    designation VARCHAR2(50),
    log_date DATE DEFAULT SYSDATE
);
CREATE OR REPLACE TRIGGER trg_employee_insert
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
    INSERT INTO employee_log
    (emp_id, emp_name, salary, dept_no, designation, log_date)
    VALUES
    (:NEW.emp_id,
     :NEW.emp_name,
     :NEW.salary,
     :NEW.dept_no,
     :NEW.designation,
     SYSDATE);
END;
/
INSERT INTO employees
(emp_id, emp_name, salary, dept_no, designation)
VALUES
(101, 'Gayathri', 5000, 10, 'Developer');
SELECT * FROM employee_log;

```
## Output:

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6198237d-e7bd-4fca-a9d8-efe565c332a1" />


**Expected Output:**
- A new entry is added to the `employee_log` table each time a new record is inserted into the `employees` table.

---

## 2. Write a trigger to prevent deletion of records from a sensitive table.
**Steps:**
- Write a **BEFORE DELETE** trigger on the `sensitive_data` table.
- Use `RAISE_APPLICATION_ERROR` to prevent deletion and issue a custom error message.
## Code:
```
CREATE TABLE sensitive_data (
    id NUMBER PRIMARY KEY,
    data_value VARCHAR2(100)
);
INSERT INTO sensitive_data
VALUES (1, 'Confidential Data');
CREATE OR REPLACE TRIGGER trg_prevent_delete
BEFORE DELETE ON sensitive_data
FOR EACH ROW
BEGIN
    RAISE_APPLICATION_ERROR(
        -20001,
        'ERROR: Deletion not allowed on this table'
    );
END;
/
DELETE FROM sensitive_data
WHERE id = 1;
```
## Output:

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/51337025-ccbc-4f62-a133-ddacf00ee9dc" />


**Expected Output:**
- If an attempt is made to delete a record from `sensitive_data`, an error message is raised, e.g., `ERROR: Deletion not allowed on this table.`

---

## 3. Write a trigger to automatically update a `last_modified` timestamp.
**Steps:**
- Add a `last_modified` column to the `products` table.
- Write a **BEFORE UPDATE** trigger on the `products` table to set the `last_modified` column to the current timestamp whenever an update occurs.

## Code:
```
-- Create table
CREATE TABLE products (
    product_id NUMBER PRIMARY KEY,
    product_name VARCHAR2(100),
    price NUMBER,
    last_modified TIMESTAMP
);

-- Insert sample record
INSERT INTO products
VALUES (1, 'Laptop', 50000, SYSTIMESTAMP);

-- Trigger to auto-update last_modified
CREATE OR REPLACE TRIGGER trg_update_last_modified
BEFORE UPDATE ON products
FOR EACH ROW
BEGIN
    :NEW.last_modified := SYSTIMESTAMP;
END;
/

-- Update record
UPDATE products
SET price = 55000
WHERE product_id = 1;

-- View result
SELECT * FROM products;
```
## Output:

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/727e10e3-525c-4826-a91c-eba538897744" />


**Expected Output:**
- The `last_modified` column in the `products` table is updated automatically to the current date and time when any record is updated.

---

## 4. Write a trigger to keep track of the number of updates made to a table.
**Steps:**
- Create an `audit_log` table with a counter column.
- Write an **AFTER UPDATE** trigger on the `customer_orders` table to increment the counter in the `audit_log` table every time a record is updated.
## Code:
```
-- Create customer_orders table
CREATE TABLE customer_orders (
    order_id NUMBER PRIMARY KEY,
    customer_name VARCHAR2(100),
    amount NUMBER
);

-- Insert sample data
INSERT INTO customer_orders VALUES (1, 'Ravi', 5000);

-- Create audit_log table
CREATE TABLE audit_log (
    update_count NUMBER
);

-- Insert initial counter value
INSERT INTO audit_log VALUES (0);

-- Trigger to count updates
CREATE OR REPLACE TRIGGER trg_count_updates
AFTER UPDATE ON customer_orders
FOR EACH ROW
BEGIN
    UPDATE audit_log
    SET update_count = update_count + 1;
END;
/

-- Update customer_orders record
UPDATE customer_orders
SET amount = 6000
WHERE order_id = 1;

-- Check counter
SELECT * FROM audit_log;
```
## Output:

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ad8f98dc-6341-4aba-b423-fcf70a213e8c" />

**Expected Output:**
- The `audit_log` table will maintain a count of how many updates have been made to the `customer_orders` table.

---

## 5. Write a trigger that checks a condition before allowing insertion into a table.
**Steps:**
- Write a **BEFORE INSERT** trigger on the `employees` table to check if the inserted salary meets a specific condition (e.g., salary must be greater than 3000).
- If the condition is not met, raise an error to prevent the insert.
## Code:
```
-- Disable old invalid trigger if exists
ALTER TRIGGER TRG_EMPLOYEE_INSERT DISABLE;

-- Create salary check trigger
CREATE OR REPLACE TRIGGER trg_check_salary
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
    IF :NEW.salary < 3000 THEN
        RAISE_APPLICATION_ERROR(
            -20001,
            'ERROR: Salary below minimum threshold'
        );
    END IF;
END;
/

-- Valid insert
INSERT INTO employees
(emp_id, emp_name, salary, dept_no, designation)
VALUES
(103, 'Gayathri', 5000, 10, 'Developer');

-- Invalid insert (will raise error)
INSERT INTO employees
(emp_id, emp_name, salary, dept_no, designation)
VALUES
(104, 'Ravi', 2500, 20, 'Tester');

-- View records
SELECT * FROM employees;

```
## Output:

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7e7a7c26-31d7-4145-907f-426e74618b2d" />


**Expected Output:**
- If the inserted salary in the `employees` table is below the condition (e.g., salary < 3000), the insert operation is blocked, and an error message is raised, such as: `ERROR: Salary below minimum threshold.`

## RESULT
Thus, the PL/SQL trigger programs were written and executed successfully.
