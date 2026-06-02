# Experiment 4: Aggregate Functions, Group By and Having Clause

## AIM
To study and implement aggregate functions, GROUP BY, and HAVING clause with suitable examples.

## THEORY

### Aggregate Functions
These perform calculations on a set of values and return a single value.

- **MIN()** – Smallest value  
- **MAX()** – Largest value  
- **COUNT()** – Number of rows  
- **SUM()** – Total of values  
- **AVG()** – Average of values

**Syntax:**
```sql
SELECT AGG_FUNC(column_name) FROM table_name WHERE condition;
```
### GROUP BY
Groups records with the same values in specified columns.
**Syntax:**
```sql
SELECT column_name, AGG_FUNC(column_name)
FROM table_name
GROUP BY column_name;
```
### HAVING
Filters the grouped records based on aggregate conditions.
**Syntax:**
```sql
SELECT column_name, AGG_FUNC(column_name)
FROM table_name
GROUP BY column_name
HAVING condition;
```

**Question 1**
--
Write a SQL query to find how many employees have an income greater than 50K?



```sql
SELECT COUNT(*) AS employees_count
FROM employee
WHERE income > 50000;
```

**Output:**

<img width="541" height="328" alt="image" src="https://github.com/user-attachments/assets/ec030744-3cc4-46ab-a346-99b105a0ad48" />


**Question 2**
---
Write a SQL query to find the youngest employee in the company?

```sql
SELECT 
    name AS Employee_Name, 
    age AS Age
FROM 
    employee
ORDER BY 
    age ASC
LIMIT 1;
```

**Output:**

<img width="813" height="353" alt="image" src="https://github.com/user-attachments/assets/eada8c10-2c9a-4b27-b3c0-d01b44d98be2" />


**Question 3**
---
Write a SQL query to find  how many employees work in California?

```sql
SELECT COUNT(*) AS employees_in_california
FROM employee
WHERE city = 'California';
```

**Output:**

<img width="683" height="326" alt="image" src="https://github.com/user-attachments/assets/9115cb24-afc4-4744-b22e-077675a75c7b" />


**Question 4**
---
How many prescriptions were written in each frequency category (e.g., once daily, twice daily)?

```sql
SELECT 
    frequency AS Frequency, 
    COUNT(*) AS TotalPrescriptions
FROM 
    Prescriptions
GROUP BY 
    frequency
ORDER BY 
    frequency ASC;
```

**Output:**

<img width="986" height="598" alt="image" src="https://github.com/user-attachments/assets/627f726b-ff63-4352-bacf-7b42aa7fcb88" />


**Question 5**
---
What is the total number of medications prescribed for each patient?

```sql
SELECT 
    PatientID, 
    COUNT(*) AS TotalMedications
FROM 
    Prescriptions
GROUP BY 
    PatientID
ORDER BY 
    PatientID ASC;
```

**Output:**

<img width="850" height="760" alt="image" src="https://github.com/user-attachments/assets/2d02a242-4e83-487d-a8f3-84e231d3e02e" />


**Question 6**
---
What is the total number of appointments scheduled by each doctor?

```sql
SELECT 
    DoctorID, 
    COUNT(*) AS TotalAppointments
FROM 
    Appointments
GROUP BY 
    DoctorID
ORDER BY 
    DoctorID ASC;
```

**Output:**

<img width="959" height="681" alt="image" src="https://github.com/user-attachments/assets/946f56d0-c206-4d69-a0a0-4354b75c44e7" />


**Question 7**
---
Write the SQL query that accomplishes the selection of average price for each category from the "products" table and includes only those products where the average price falls between 10 and 15.

```sql
SELECT 
    category_id, 
    AVG(Price)
FROM 
    products
GROUP BY 
    category_id
HAVING 
    AVG(Price) BETWEEN 10 AND 15;
```

**Output:**

<img width="721" height="455" alt="image" src="https://github.com/user-attachments/assets/ea19a7ed-f593-4106-bb15-070f71bf0146" />


**Question 8**
---
Write the SQL query that achieves the grouping of data by occupation, calculates the total work hours for each occupation, and excludes occupations where the total work hour sum is not greater than 20.

```sql
SELECT 
    occupation, 
    SUM(workhour)
FROM 
    employee1
GROUP BY 
    occupation
HAVING 
    SUM(workhour) > 20;
```

**Output:**

<img width="687" height="512" alt="image" src="https://github.com/user-attachments/assets/6f26c732-9c86-47c3-aa99-3281ef41b3a4" />



**Question 9**
---
Write the SQL query that achieves the grouping of data by occupation, calculates the minimum work hours for each occupation, and excludes occupations where the minimum work hour is not greater than 8.

```sql
SELECT 
    occupation, 
    MIN(workhour)
FROM 
    employee1
GROUP BY 
    occupation
HAVING 
    MIN(workhour) > 8;
```

**Output:**

<img width="678" height="529" alt="image" src="https://github.com/user-attachments/assets/b02071a8-549d-4671-8762-22e3147fec98" />


**Question 10**
---
Write the SQL query to find how many patients have more than 3 medical records?.

```sql
SELECT 
    PatientID, 
    COUNT(*) AS TotalRecords
FROM 
    MedicalRecords
GROUP BY 
    PatientID
HAVING 
    COUNT(*) > 3;
```

**Output:**

<img width="803" height="486" alt="image" src="https://github.com/user-attachments/assets/e35c2855-53b0-48bf-94b8-8129262436b1" />



## RESULT
Thus, the SQL queries to implement aggregate functions, GROUP BY, and HAVING clause have been executed successfully.
