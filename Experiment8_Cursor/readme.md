# Experiment 8: PL/SQL Cursor Programs

## AIM
To write and execute PL/SQL programs using cursors and exception handling to manage runtime errors effectively and display appropriate messages.

## THEORY

In PL/SQL, cursors are used to handle query result sets row-by-row. 

There are two types of cursors:

- Implicit Cursors: Automatically created by PL/SQL for single-row queries.
- Explicit Cursors: Declared and controlled by the programmer for multi-row queries.

Types of Explicit Cursors:

1. Simple Cursor: Basic cursor to iterate over multiple rows.

2. Parameterized Cursor: Accepts parameters to filter the result dynamically.

3. Cursor FOR Loop: Simplifies cursor operations (open, fetch, close).

4. %ROWTYPE Cursor: Fetches entire row into a record using %ROWTYPE.

5. Cursor with FOR UPDATE: Used for row-level locking and updating the rows while looping.

**Syntax:**
```sql
DECLARE 
   <declarations section> 
BEGIN 
   <executable command(s)>
EXCEPTION 
   <exception handling> 
END;
```

### Basic Components of PL/SQL Block:

- DECLARE: Section to declare variables and constants.
- BEGIN: The execution section that contains PL/SQL statements.
- EXCEPTION: Handles errors or exceptions that occur in the program.
- END: Marks the end of the PL/SQL block.

**Exception Handling**

PL/SQL provides a robust mechanism to handle runtime errors using exception handling blocks. When an error occurs during execution, control is passed to the EXCEPTION section, where specific or general errors can be handled gracefully.

### Components of Exception Handling:
- Predefined Exceptions: Automatically raised by PL/SQL for common errors (e.g., NO_DATA_FOUND, TOO_MANY_ROWS, ZERO_DIVIDE).
- User-defined Exceptions: Declared explicitly in the declaration section using the EXCEPTION keyword.
- WHEN OTHERS: A generic handler for all exceptions not handled explicitly.

```sql
BEGIN
   -- Statements
EXCEPTION
   WHEN exception_name THEN
      -- Handling code
   WHEN OTHERS THEN
      -- Handling for unknown errors
END;
```

### **Question 1: Simple Cursor with Exception Handling**

**Write a PL/SQL program using a simple cursor to fetch employee names and designations from the `employees` table. Implement exception handling for the following cases:**

1. **NO_DATA_FOUND**: When no rows are fetched.
2. **OTHERS**: Any other unexpected errors during execution.

**Steps:**

- Create an `employees` table with fields `emp_id`, `emp_name`, and `designation`.
- Insert some sample data into the table.
- Use a simple cursor to fetch and display employee names and designations.
- Implement exception handling to catch the relevant exceptions and display appropriate messages.
## Code:
```

-- Step 1: Create employees table
CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50),
    designation VARCHAR2(50)
);

-- Step 2: Insert sample data
INSERT INTO employees VALUES (1, 'Arun', 'Manager');
INSERT INTO employees VALUES (2, 'Priya', 'Developer');
INSERT INTO employees VALUES (3, 'Karthik', 'Tester');

COMMIT;

-- Step 3 & 4: PL/SQL program using simple cursor with exception handling

DECLARE
   CURSOR emp_cur IS
      SELECT emp_name, designation FROM employees;
   v_name employees.emp_name%TYPE;
   v_desg employees.designation%TYPE;
BEGIN
   OPEN emp_cur;
   LOOP
      FETCH emp_cur INTO v_name, v_desg;
      EXIT WHEN emp_cur%NOTFOUND;
      DBMS_OUTPUT.PUT_LINE('Name: ' || v_name || ', Designation: ' || v_desg);
   END LOOP;
   CLOSE emp_cur;
EXCEPTION
   WHEN OTHERS THEN
      DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
END;

```




**Output:**  
The program should display the employee details or an error message.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/01b1a33a-3664-4d65-9943-6dcf3d2816ac" />


---

### **Question 2: Parameterized Cursor with Exception Handling**

**Write a PL/SQL program using a parameterized cursor to retrieve and display employees with a salary in a given range. Implement exception handling for the following errors:**

1. **NO_DATA_FOUND**: When no employees meet the salary criteria.
2. **OTHERS**: For any unexpected errors during the execution.

**Steps:**

- Modify the `employees` table by adding a `salary` column.
- Insert sample salary values for the employees.
- Use a parameterized cursor to accept a salary range as input and fetch employees within that range.
- Implement exception handling to catch and display relevant error messages.

## Code:
```
-- Step 1: Modify employees table by adding salary column
ALTER TABLE employees
ADD salary NUMBER(10,2);

-- Step 2: Insert sample salary values
UPDATE employees
SET salary = 50000
WHERE emp_id = 1;

UPDATE employees
SET salary = 35000
WHERE emp_id = 2;

UPDATE employees
SET salary = 28000
WHERE emp_id = 3;

COMMIT;

-- Step 3 & 4: PL/SQL program using parameterized cursor
DECLARE
    v_min_salary NUMBER := 30000;
    v_max_salary NUMBER := 60000;

    v_name employees.emp_name%TYPE;
    v_designation employees.designation%TYPE;
    v_salary employees.salary%TYPE;

    CURSOR emp_cursor(p_min NUMBER, p_max NUMBER) IS
        SELECT emp_name, designation, salary
        FROM employees
        WHERE salary BETWEEN p_min AND p_max;

BEGIN
    OPEN emp_cursor(v_min_salary, v_max_salary);

    FETCH emp_cursor INTO v_name, v_designation, v_salary;

    IF emp_cursor%NOTFOUND THEN
        RAISE NO_DATA_FOUND;
    END IF;

    WHILE emp_cursor%FOUND LOOP
        DBMS_OUTPUT.PUT_LINE(
            'Employee Name: ' || v_name ||
            ' | Designation: ' || v_designation ||
            ' | Salary: ' || v_salary
        );

        FETCH emp_cursor INTO v_name, v_designation, v_salary;
    END LOOP;

    CLOSE emp_cursor;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No employees found in the given salary range.');

    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('An unexpected error occurred: ' || SQLERRM);
END;




```


**Output:**  
The program should display the employee details within the specified salary range or an error message if no data is found.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/624fcc2a-668b-427d-ae1c-3ab10698f78b" />


---

### **Question 3: Cursor FOR Loop with Exception Handling**

**Write a PL/SQL program using a cursor FOR loop to retrieve and display all employee names and their department numbers from the `employees` table. Implement exception handling for the following cases:**

1. **NO_DATA_FOUND**: If no employees are found in the database.
2. **OTHERS**: For any other unexpected errors.

**Steps:**

- Modify the `employees` table by adding a `dept_no` column.
- Insert sample department numbers for employees.
- Use a cursor FOR loop to fetch and display employee names along with their department numbers.
- Implement exception handling to catch the relevant exceptions.

## Code:
```
-- Step 1: Modify employees table by adding dept_no column
ALTER TABLE employees
ADD dept_no NUMBER(5);

-- Step 2: Insert sample department numbers
UPDATE employees
SET dept_no = 101
WHERE emp_id = 1;

UPDATE employees
SET dept_no = 102
WHERE emp_id = 2;

UPDATE employees
SET dept_no = 103
WHERE emp_id = 3;

COMMIT;

-- Step 3 & 4: PL/SQL program using Cursor FOR Loop
DECLARE
    v_count NUMBER;

BEGIN
    -- Check if records exist
    SELECT COUNT(*) INTO v_count
    FROM employees;

    IF v_count = 0 THEN
        RAISE NO_DATA_FOUND;
    END IF;

    -- Cursor FOR Loop
    FOR emp_rec IN (
        SELECT emp_name, dept_no
        FROM employees
    )
    LOOP
        DBMS_OUTPUT.PUT_LINE(
            'Employee Name: ' || emp_rec.emp_name ||
            ' | Department No: ' || emp_rec.dept_no
        );
    END LOOP;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('No employees found in the database.');

    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('An unexpected error occurred: ' || SQLERRM);
END;
/

```

**Output:**  
The program should display employee names with their department numbers or the appropriate error message if no data is found.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c6c16166-0c67-43c9-b5eb-6899076a1d23" />

---

### **Question 4: Cursor with `%ROWTYPE` and Exception Handling**

**Write a PL/SQL program that uses a cursor with `%ROWTYPE` to fetch and display complete employee records (emp_id, emp_name, designation, salary). Implement exception handling for the following errors:**

1. **NO_DATA_FOUND**: When no employees are found in the database.
2. **OTHERS**: For any other errors that occur.

**Steps:**

- Modify the `employees` table by adding `emp_id`, `emp_name`, `designation`, and `salary` fields.
- Insert sample data into the `employees` table.
- Declare a cursor using `%ROWTYPE` to fetch complete rows from the `employees` table.
- Implement exception handling to catch the relevant exceptions and display appropriate messages.
## Code:
```
-- Create employees table
CREATE TABLE employees (
    emp_id NUMBER PRIMARY KEY,
    emp_name VARCHAR2(50),
    designation VARCHAR2(50),
    salary NUMBER(10,2)
);

-- Insert sample data
INSERT INTO employees VALUES (1, 'Arun', 'Manager', 50000);
INSERT INTO employees VALUES (2, 'Priya', 'Developer', 35000);
INSERT INTO employees VALUES (3, 'Karthik', 'Tester', 28000);

COMMIT;

-- PL/SQL Program using %ROWTYPE Cursor

DECLARE
   CURSOR emp_cur IS
      SELECT * FROM employees;

   emp_rec employees%ROWTYPE;
   v_count NUMBER;

BEGIN

   -- Check whether records exist
   SELECT COUNT(*) INTO v_count
   FROM employees;

   IF v_count = 0 THEN
      RAISE NO_DATA_FOUND;
   END IF;

   OPEN emp_cur;

   LOOP
      FETCH emp_cur INTO emp_rec;
      EXIT WHEN emp_cur%NOTFOUND;

      DBMS_OUTPUT.PUT_LINE(
         'Employee ID: ' || emp_rec.emp_id ||
         ' | Name: ' || emp_rec.emp_name ||
         ' | Designation: ' || emp_rec.designation ||
         ' | Salary: ' || emp_rec.salary
      );

   END LOOP;

   CLOSE emp_cur;

EXCEPTION
   WHEN NO_DATA_FOUND THEN
      DBMS_OUTPUT.PUT_LINE('No employees found in the database.');

   WHEN OTHERS THEN
      DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);

END;
/
```

**Output:**  
The program should display employee records or the appropriate error message if no data is found.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ca96e306-00b0-4861-addd-0d04c8691e11" />


---

### **Question 5: Cursor with FOR UPDATE Clause and Exception Handling**

**Write a PL/SQL program using a cursor with the `FOR UPDATE` clause to update the salary of employees in a specific department. Implement exception handling for the following cases:**

1. **NO_DATA_FOUND**: If no rows are affected by the update.
2. **OTHERS**: For any unexpected errors during execution.

**Steps:**

- Modify the `employees` table to include a `dept_no` and `salary` field.
- Insert sample data into the `employees` table with different department numbers.
- Use a cursor with the `FOR UPDATE` clause to lock the rows of employees in a specific department and update their salary.
- Implement exception handling to handle `NO_DATA_FOUND` or other errors that may occur.
## Code:
```
-- Add dept_no column if not already present
ALTER TABLE employees
ADD dept_no NUMBER(5);

-- Insert / update sample data
UPDATE employees
SET dept_no = 101, salary = 50000
WHERE emp_id = 1;

UPDATE employees
SET dept_no = 102, salary = 35000
WHERE emp_id = 2;

UPDATE employees
SET dept_no = 101, salary = 28000
WHERE emp_id = 3;

COMMIT;

-- PL/SQL Program using FOR UPDATE Cursor

DECLARE
   CURSOR emp_cur IS
      SELECT emp_id, emp_name, salary
      FROM employees
      WHERE dept_no = 101
      FOR UPDATE;

   v_empid employees.emp_id%TYPE;
   v_name employees.emp_name%TYPE;
   v_salary employees.salary%TYPE;

   v_count NUMBER := 0;

BEGIN
   OPEN emp_cur;

   LOOP
      FETCH emp_cur INTO v_empid, v_name, v_salary;
      EXIT WHEN emp_cur%NOTFOUND;

      UPDATE employees
      SET salary = salary + 5000
      WHERE CURRENT OF emp_cur;

      DBMS_OUTPUT.PUT_LINE(
         'Employee: ' || v_name ||
         ' | Updated Salary: ' || (v_salary + 5000)
      );

      v_count := v_count + 1;

   END LOOP;

   CLOSE emp_cur;

   IF v_count = 0 THEN
      RAISE NO_DATA_FOUND;
   END IF;

   COMMIT;

EXCEPTION
   WHEN NO_DATA_FOUND THEN
      DBMS_OUTPUT.PUT_LINE('No employees found in the specified department.');

   WHEN OTHERS THEN
      DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);

END;
/
```

**Output:**  
The program should update employee salaries and display a message, or it should display an error message if no data is found.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cc782485-110f-4f18-97a5-6cf86f258bda" />

---

## RESULT
Thus, the program successfully executed and displayed employee details using a cursor. 

