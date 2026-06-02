# Experiment 3: DML Commands

## AIM
To study and implement DML (Data Manipulation Language) commands.

## THEORY

### 1. INSERT INTO
Used to add records into a relation.
These are three type of INSERT INTO queries which are as
A)Inserting a single record
**Syntax (Single Row):**
```sql
INSERT INTO table_name (field_1, field_2, ...) VALUES (value_1, value_2, ...);
```
**Syntax (Multiple Rows):**
```sql
INSERT INTO table_name (field_1, field_2, ...) VALUES
(value_1, value_2, ...),
(value_3, value_4, ...);
```
**Syntax (Insert from another table):**
```sql
INSERT INTO table_name SELECT * FROM other_table WHERE condition;
```
### 2. UPDATE
Used to modify records in a relation.
Syntax:
```sql
UPDATE table_name SET column1 = value1, column2 = value2 WHERE condition;
```
### 3. DELETE
Used to delete records from a relation.
**Syntax (All rows):**
```sql
DELETE FROM table_name;
```
**Syntax (Specific condition):**
```sql
DELETE FROM table_name WHERE condition;
```
### 4. SELECT
Used to retrieve records from a table.
**Syntax:**
```sql
SELECT column1, column2 FROM table_name WHERE condition;
```
**Question 1**
--
Write a SQL query to calculate the absolute value of the value1 column from the Calculations table.

```sql
select id,value1, ABS (value1)  as  absolute_value from Calculations;

```

**Output:**

<img width="893" height="318" alt="image" src="https://github.com/user-attachments/assets/771fc185-cb21-427a-bf58-47ede2e76632" />


**Question 2**
---
Write a SQL query to find the exact date that is 100 days after each employee's hire date.

```sql
select ename ,hiredate,DATE(hiredate,'+100 days') as  DateAfter100Days from emp;
```

**Output:**

<img width="868" height="357" alt="image" src="https://github.com/user-attachments/assets/98449ae3-7301-4e17-9b03-82aa04b0a056" />


**Question 3**
---
Write a SQL query to select orders between 500 and 4000 (begin and end values are included). Exclude orders amount 948.50 and 1983.43. Return ord_no, purch_amt, ord_date, customer_id, and salesman_id.
```sql
SELECT ord_no, purch_amt, ord_date, customer_id, salesman_id
FROM orders
WHERE purch_amt BETWEEN 500 AND 4000
AND purch_amt NOT IN (948.50, 1983.43);
```

**Output:**

<img width="1185" height="398" alt="image" src="https://github.com/user-attachments/assets/a53dd73b-8566-4e10-99e7-e129ee140dab" />


**Question 4**
---
 Write a query to fetch 3 top salaried records from EmployeePosition table.

```sql
select EmpID , EmpPosition,DateOfJoining,Salary from EmployeePosition order by Salary desc limit 3;

```

**Output:**

<img width="1188" height="344" alt="image" src="https://github.com/user-attachments/assets/5d0728dd-3ae9-4ba6-8118-2cead346f850" />


**Question 5**
---
Write a SQL query to retrieve the year, month, and day from the hiredate column in the emp table.

```sql
SELECT 
    strftime('%Y', hiredate) AS Year,
    strftime('%m', hiredate) AS Month,
    strftime('%d', hiredate) AS Day
FROM emp;
```

**Output:**

<img width="862" height="386" alt="image" src="https://github.com/user-attachments/assets/ee8f4241-95f7-46dc-9bd6-d83a3e1632a8" />


**Question 6**
---
Write a SQL query to Delete a Specific Surgery whose ID is 3 or surgeon ID is 4.
```sql
delete from Surgeries where surgeon_id=3
 or surgeon_id =4;
```

**Output:**

<img width="1177" height="840" alt="image" src="https://github.com/user-attachments/assets/873b9447-00ad-4a48-8b55-5302e324ef2a" />


**Question 7**
---
Change the supplier name to upper case where contact person contains ' Singh' in suppliers table.

```sql
UPDATE suppliers
SET supplier_name = UPPER(supplier_name)
WHERE contact_person LIKE '% Singh%';
```

**Output:**

<img width="1194" height="356" alt="image" src="https://github.com/user-attachments/assets/29ff1e00-f7f8-466f-9f91-9f2db6301f52" />


**Question 8**
---
Write a SQL query to calculate the final price after applying both the discount and the tax. Return product_id, original_price, discount_percentage, tax_rate, and final_price.

```sql
SELECT 
    product_id,
    original_price,
    discount_percentage,
    tax_rate,
    original_price * (1 - discount_percentage) * (1 + tax_rate) AS final_price
FROM Products;
```

**Output:**

<img width="1221" height="357" alt="image" src="https://github.com/user-attachments/assets/d4f17293-f847-4c75-93fd-e69d852ae53c" />


**Question 9**
---
Write a SQL query to Delete customers from 'customer' table where 'GRADE' is exactly 2.
```sql
delete from Customer where GRADE =2;
```

**Output:**

<img width="856" height="582" alt="image" src="https://github.com/user-attachments/assets/30ecf107-b369-415a-98b3-a9f4305bff66" />


**Question 10**
---
Write a SQL query to Delete customers from 'customer' table where 'GRADE' is not equal to 3.

```sql
delete from Customer where GRADE !=3;
```

**Output:**

<img width="771" height="495" alt="image" src="https://github.com/user-attachments/assets/b3bffa03-2350-443c-91e0-e512ecc25b52" />


## RESULT
Thus, the SQL queries to implement DML commands have been executed successfully.
