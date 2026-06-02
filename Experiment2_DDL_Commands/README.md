# Experiment 2: DDL Commands

## AIM
To study and implement DDL commands and different types of constraints.

## THEORY

### 1. CREATE
Used to create a new relation (table).

**Syntax:**
```sql
CREATE TABLE (
  field_1 data_type(size),
  field_2 data_type(size),
  ...
);
```
### 2. ALTER
Used to add, modify, drop, or rename fields in an existing relation.
(a) ADD
```sql
ALTER TABLE std ADD (Address CHAR(10));
```
(b) MODIFY
```sql
ALTER TABLE relation_name MODIFY (field_1 new_data_type(size));
```
(c) DROP
```sql
ALTER TABLE relation_name DROP COLUMN field_name;
```
(d) RENAME
```sql
ALTER TABLE relation_name RENAME COLUMN old_field_name TO new_field_name;
```
### 3. DROP TABLE
Used to permanently delete the structure and data of a table.
```sql
DROP TABLE relation_name;
```
### 4. RENAME
Used to rename an existing database object.
```sql
RENAME TABLE old_relation_name TO new_relation_name;
```
### CONSTRAINTS
Constraints are used to specify rules for the data in a table. If there is any violation between the constraint and the data action, the action is aborted by the constraint. It can be specified when the table is created (using CREATE TABLE) or after it is created (using ALTER TABLE).
### 1. NOT NULL
When a column is defined as NOT NULL, it becomes mandatory to enter a value in that column.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) NOT NULL
);
```
### 2. UNIQUE
Ensures that values in a column are unique.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) UNIQUE
);
```
### 3. CHECK
Specifies a condition that each row must satisfy.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) CHECK (logical_expression)
);
```
### 4. PRIMARY KEY
Used to uniquely identify each record in a table.
Properties:
Must contain unique values.
Cannot be null.
Should contain minimal fields.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) PRIMARY KEY
);
```
### 5. FOREIGN KEY
Used to reference the primary key of another table.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size),
  FOREIGN KEY (column_name) REFERENCES other_table(column)
);
```
### 6. DEFAULT
Used to insert a default value into a column if no value is specified.

Syntax:
```sql
CREATE TABLE Table_Name (
  col_name1 data_type,
  col_name2 data_type,
  col_name3 data_type DEFAULT 'default_value'
);
```

**Question 1**
--
Create a table named Products with the following columns:

ProductID as INTEGER
ProductName as TEXT
Price as REAL
Stock as INTEGER

```sql
create table products(
ProductID INTEGER,
ProductName TEXT,
Price REAL,
Stock INTEGER
);
```

**Output:**

<img width="1205" height="366" alt="image" src="https://github.com/user-attachments/assets/2b81155b-8d8b-4bbd-a173-7aa3d5cf780e" />


**Question 2**
---
Write an SQL Query to add the attributes designation, net_salary, and dob to the Companies table with the following data types:
designation as VARCHAR(50)
net_salary as NUMBER
dob as DATE

```sql
alter table Companies add column designation varchar(50);
alter table Companies add column net_salary number;
alter table Companies add column dob date;
```

**Output:**

<img width="1185" height="401" alt="image" src="https://github.com/user-attachments/assets/49adaff9-75eb-4d96-aa91-5032c1075970" />


**Question 3**
---
Create a table named Locations with the following columns:

LocationID as INTEGER
LocationName as TEXT
Address as TEXT

```sql
create table Locations(
LocationID INTEGER,
LocationName TEXT,
Address TEXT
);
```

**Output:**

<img width="1179" height="369" alt="image" src="https://github.com/user-attachments/assets/f2df3e1f-733a-468d-9957-1c90a9956551" />


**Question 4**
---
Create a table named Orders with the following constraints:
OrderID as INTEGER should be the primary key.
OrderDate as DATE should be not NULL.
CustomerID as INTEGER should be a foreign key referencing Customers(CustomerID).

```sql
create table Orders(
OrderID INTEGER primary key,
OrderDate DATE not null,
CustomerID INTEGER,
foreign key (CustomerID) references Customers(CustomerID)
);
```

**Output:**

<img width="1188" height="272" alt="image" src="https://github.com/user-attachments/assets/f008ade9-1208-4c77-b4a8-a2a040887cab" />


**Question 5**
---
Create a table named Events with the following columns:

EventID as INTEGER
EventName as TEXT
EventDate as DATE

```sql
create table Events(
EventID INTEGER,
EventName TEXT,
EventDate DATE
);
```

**Output:**

<img width="1192" height="366" alt="image" src="https://github.com/user-attachments/assets/7a1111b9-6cf8-40fb-b1fb-52fa44573f0c" />


**Question 6**
---
Create a table named Orders with the following columns:

OrderID as INTEGER
OrderDate as TEXT
CustomerID as INTEGER

```sql
create table Orders(
OrderID INTEGER,
OrderDate TEXT,
CustomerID INTEGER
);
```

**Output:**

<img width="1179" height="363" alt="image" src="https://github.com/user-attachments/assets/792261df-8899-4dd2-b50d-882bd99467f1" />


**Question 7**
---
Insert the following products into the Products table:

```sql
insert into Products (Name,Category,Price,Stock)
 values
 ('Smartphone','Electronics',800,150),
 ('Headphones','Accessories',200,300);
```

**Output:**

<img width="1190" height="342" alt="image" src="https://github.com/user-attachments/assets/7bb6aa63-a135-4f8f-8fda-d5ee8ef433f3" />


**Question 8**
---
Insert a student with RollNo 201, Name David Lee, Gender M, Subject Physics, and MARKS 92 into the Student_details table.

```sql
insert into Student_details (RollNo,Name,Gender,Subject,Marks)
values 
(201,'David Lee','M','Physics',92);
```

**Output:**

<img width="1186" height="234" alt="image" src="https://github.com/user-attachments/assets/748ddc56-978d-4b9b-88cd-1bc11998d1ba" />


**Question 9**
---
Write a SQL query to modify the Student_details table by adding a new column Email of type VARCHAR(50) and updating the column MARKS to have a default value of 0.

```sql

ALTER table Student_details add Email varchar(50);
alter table Student_details 
add column MARKS integer default 0;
```

**Output:**

<img width="1180" height="231" alt="image" src="https://github.com/user-attachments/assets/428cbeda-3bb3-47c1-9063-3156ed1dd224" />


**Question 10**
---
Insert all students from Archived_students table into the Student_details table.

```sql
insert into Student_details(RollNo,Name,Gender,Subject,Marks)
select RollNo,Name,Gender,Subject,Marks
from Archived_students;
```

**Output:**

<img width="1181" height="276" alt="image" src="https://github.com/user-attachments/assets/fb8987d6-e47c-40fe-9a18-bfd3a12fa01f" />



## RESULT
Thus, the SQL queries to implement different types of constraints and DDL commands have been executed successfully.
