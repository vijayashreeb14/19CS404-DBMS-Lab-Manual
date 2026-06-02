# Experiment 6: Joins

## AIM
To study and implement different types of joins.

## THEORY

SQL Joins are used to combine records from two or more tables based on a related column.

### 1. INNER JOIN
Returns records with matching values in both tables.

**Syntax:**
```sql
SELECT columns
FROM table1
INNER JOIN table2
ON table1.column = table2.column;
```

### 2. LEFT JOIN
Returns all records from the left table, and matched records from the right.

**Syntax:**

```sql
SELECT columns
FROM table1
LEFT JOIN table2
ON table1.column = table2.column;
```
### 3. RIGHT JOIN
Returns all records from the right table, and matched records from the left.

**Syntax:**

```sql
SELECT columns
FROM table1
RIGHT JOIN table2
ON table1.column = table2.column;
```
### 4. FULL OUTER JOIN
Returns all records when there is a match in either left or right table.

**Syntax:**

```sql
SELECT columns
FROM table1
FULL OUTER JOIN table2
ON table1.column = table2.column;
```

**Question 1**
--
write a SQL query to find the salesperson and customer who reside in the same city. Return Salesman, cust_name and city.

```sql
SELECT 
    salesman.name AS "Salesman", 
    customer.cust_name, 
    salesman.city
FROM salesman
INNER JOIN customer 
    ON salesman.city = customer.city;
```

**Output:**

<img width="1187" height="677" alt="image" src="https://github.com/user-attachments/assets/ac2fec5e-b3f4-47dc-bcba-9ba758382fa4" />


**Question 2**
---
Write a SQL statement to make a report with customer name, city, order number, order date, and order amount in ascending order according to the order date to determine whether any of the existing customers have placed an order or not.

```sql
SELECT 
    c.cust_name, 
    c.city, 
    o.ord_no, 
    o.ord_date, 
    o.purch_amt AS "Order Amount"
FROM customer c
LEFT OUTER JOIN orders o 
    ON c.customer_id = o.customer_id
ORDER BY o.ord_date ASC;
```

**Output:**

<img width="1205" height="793" alt="image" src="https://github.com/user-attachments/assets/f48229e2-3c04-4d6c-835c-7e7a068721a2" />


**Question 3**
---
Write the SQL query that achieves the selection of the first name from the "patients" table (aliased as "patient_name") and all columns from the "test_results" table (aliased as "t"), with an inner join on the "patient_id" column and a condition filtering for patients admitted between '2024-01-01' and '2024-01-31'.

```sql
select p.first_name as patient_name,t.*
from PATIENTS p
inner join TEST_RESULTS t
on t.patient_id=p.patient_id
where admission_date between '2024-01-01' and '2024-01-31';
```

**Output:**

<img width="1185" height="417" alt="image" src="https://github.com/user-attachments/assets/c3c46f58-a604-4688-b15a-44f0a72f5469" />

**Question 4**
---
Write the SQL query that achieves the selection of the first name from the "patients" table (aliased as "patient_name") and the first name from the "doctors" table (aliased as "doctor_name"), with an inner join on the "doctor_id" column and a condition filtering for patients with a null discharge date.

```sql
select p.first_name as patient_name,d.first_name as doctor_name
from PATIENTS p
inner join DOCTORS d
on d.doctor_id = p.doctor_id
where discharge_date is NULL;
```

**Output:**


<img width="732" height="465" alt="image" src="https://github.com/user-attachments/assets/0b6daa49-3a08-4a80-b74d-9dc2b3c2afe1" />


**Question 5**
---
Write the SQL query that achieves the selection of the first name from the "patients" table, with an inner join on the "patient_id" column and a condition filtering for surgeries with a surgery date of '2024-01-15'.:

```sql
select p.first_name
from PATIENTS p
inner join SURGERIES s
on s.patient_id = p.patient_id
where surgery_date ='2024-01-15';
```

**Output:**

<img width="474" height="414" alt="image" src="https://github.com/user-attachments/assets/d1a2bcda-0520-46d6-9636-169a7ce3d544" />


**Question 6**
---
 From the following tables write a SQL query to find the salesperson(s) and the customer(s) he represents. Return Customer Name, city, Salesman, commission.

```sql
SELECT c.cust_name AS "Customer Name",
       c.city,
       s.name AS Salesman,
       s.commission
FROM customer c
JOIN salesman s
ON c.salesman_id = s.salesman_id;
```

**Output:**

<img width="1182" height="823" alt="image" src="https://github.com/user-attachments/assets/ac4f658c-1ab2-4f56-abfb-e6aabee81323" />


**Question 7**
---
Write the SQL query that achieves the selection of the "nurse_id" from the "nurses" table (aliased as "n") and the "department_name" from the "departments" table, with an inner join on the "department_id" column and conditions filtering for a nurse with the first name 'David' and last name 'Moore'.

```sql
select n.nurse_id,d.department_name
from NURSES n
inner join DEPARTMENTS d
on d.department_id = n.department_id
where first_name='David' and last_name='Moore';
```

**Output:**

<img width="767" height="399" alt="image" src="https://github.com/user-attachments/assets/25d15fa4-75f5-4a0e-bee5-732500647d6c" />


**Question 8**
---
Write the SQL query that achieves the selection of the "cust_name" column from the "customer" table (aliased as "c"), and the "ord_no," "ord_date," and "purch_amt" columns from the "orders" table (aliased as "o"), with a left join on the "customer_id" column and a condition filtering for orders with a purchase amount greater than 1000.

```sql
select c.cust_name,o.ord_no,o.ord_date,o.purch_amt
from CUSTOMER c
left join ORDERS o
on o.customer_id = c.customer_id
where purch_amt > 1000;
```

**Output:**

<img width="1187" height="707" alt="image" src="https://github.com/user-attachments/assets/46ab0d68-56c5-4a0d-9840-78be86206e75" />


**Question 9**
---
Write the SQL query that achieves the selection of the first name from the "patients" table (aliased as "patient_name") and the specialization from the "doctors" table (aliased as "Doctor_specialization"), with an inner join on the "doctor_id" column and a condition filtering for patients admitted between '2024-01-01' and '2024-01-31'.

```sql
select p.first_name as patient_name,d.specialization as Doctor_specialist
from PATIENTS p
inner join DOCTORS d
on p.doctor_id=d.doctor_id
where admission_date between '2024-01-01' and'2024-01-31';
```

**Output:**

<img width="777" height="428" alt="image" src="https://github.com/user-attachments/assets/7ff382d4-30bc-495f-b4c3-56739a834873" />


**Question 10**
---
Write the SQL query that achieves the selection of all columns from the "salesman" table (aliased as "s"), with a left join on the "salesman_id" column and a condition filtering for customers with the name 'Fabian Johns'.

```sql
select s.*
from Salesman s
left join Customer c
on s.salesman_id=c.salesman_id
where c.cust_name='Fabian Johns';
```

**Output:**

<img width="1212" height="464" alt="image" src="https://github.com/user-attachments/assets/f3cf4d4d-783f-4cd9-adae-539baa53581a" />



## RESULT
Thus, the SQL queries to implement different types of joins have been executed successfully.
