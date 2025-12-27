Below is a **PostgreSQL version that mirrors exactly how you taught MongoDB** — same **flow, clarity, 1–2 line theory, why we use it, and runnable commands**.
This is **perfect for a 2.5–3 hr SQL class** and helps students **mentally map SQL ↔ MongoDB**.

---

# 🟦 PostgreSQL (SQL) – Beginner to Practical Walkthrough

![Image](https://docs.netapp.com/us-en/ontap-apps-dbs/media/postgresql-architecture.png)

![Image](https://s33046.pcdn.co/wp-content/uploads/2018/12/word-image-228.png)

![Image](https://media.enlabsoftware.com/wp-content/uploads/2021/03/17085823/Diagrams-to-SQL-Server-Indexes-e1615946303564.png)

We’ll assume **PostgreSQL (`psql`) CLI** or **pgAdmin**.

---

## 🟢 Step 1: Database-Level Commands

### 1️⃣ Show Databases

**What it does:**
Lists all databases available in PostgreSQL.

**Why we use it:**
To check existing environments (dev, test, prod).

```sql
\l
```

---

### 2️⃣ Create Database

**What it does:**
Creates a new database.

**Why we use it:**
Each project/app usually has its own database.

```sql
CREATE DATABASE company_db;
```

---

### 3️⃣ Connect to Database

**What it does:**
Switches to the selected database.

**Why we use it:**
All tables and queries run inside a database.

```sql
\c company_db;
```

---

### 4️⃣ Drop Database

**What it does:**
Deletes the database permanently.

**Why we use it:**
Reset during development/testing.

```sql
DROP DATABASE company_db;
```

---

## 🟢 Step 2: Create Table (Schema Definition)

> ⚠️ **Big SQL Difference vs MongoDB**
> SQL requires **schema before data**.

### Create Table

**What it does:**
Defines structure, data types, and constraints.

**Why we use it:**
Ensures data consistency and integrity.

```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100),
  age INT,
  phone_number BIGINT,
  salary NUMERIC(10,2),
  is_active BOOLEAN,
  skills TEXT[],
  created_at TIMESTAMP,
  address JSONB
);
```

---

## 🟢 Step 3: Insert Data (Single + Bulk)

### Insert One Record

**What it does:**
Inserts a single row.

**Why we use it:**
Single user signup / admin entry.

```sql
INSERT INTO users (name)
VALUES ('Sahil');
```

---

## 🟢 Step 4: ONE `INSERT` to Set Up Everything

> This **single bulk insert supports ALL queries below**

```sql
INSERT INTO users
(name, age, phone_number, salary, is_active, skills, created_at, address)
VALUES
(
  'Sahil', 25, 8431185185, 55000.75, true,
  ARRAY['React','Node'],
  NOW(),
  '{"city":"Hyderabad","pincode":500081}'
),
(
  'Amit', 23, 8431185195, 42000, false,
  ARRAY['Java'],
  '2024-01-10',
  '{"city":"Bangalore","pincode":560001}'
),
(
  'Rohit', 21, NULL, 38000, true,
  ARRAY['Python'],
  '2023-11-15',
  '{"city":"Pune","pincode":411001}'
),
(
  'Neha', 26, 9898180180, NULL, true,
  ARRAY[]::TEXT[],
  NOW(),
  '{"city":"Mumbai","pincode":400001}'
);
```

---

## 🟢 Step 5: Read (SELECT)

### View All Records

**What it does:**
Fetches all rows from table.

**Why we use it:**
Verify inserted data.

```sql
SELECT * FROM users;
```

---

## 🟢 Data Types (SQL Version)

| Type      | Example      |
| --------- | ------------ |
| VARCHAR   | `name`       |
| INT       | `age`        |
| NUMERIC   | `salary`     |
| BOOLEAN   | `is_active`  |
| ARRAY     | `skills`     |
| TIMESTAMP | `created_at` |
| NULL      | `salary`     |
| JSONB     | `address`    |

---

## 🟢 Sorting

**What it does:**
Orders result set.

**Why we use it:**
Alphabetical lists, latest records.

```sql
SELECT * FROM users ORDER BY name DESC;
```

---

## 🟢 Limit

**What it does:**
Restricts number of rows returned.

**Why we use it:**
Pagination.

```sql
SELECT * FROM users LIMIT 3;
```

---

## 🟢 Filtering with WHERE

```sql
SELECT * FROM users
WHERE name = 'Sahil'
AND phone_number = 8431185185;
```

**Why we use it:**
Precise record lookup.

---

## 🟢 Projection (Selecting Columns)

**What it does:**
Fetches only required columns.

**Why we use it:**
Reduce payload & improve performance.

```sql
SELECT name, phone_number FROM users;
```

---

## 🟢 Update Records

### Update One

```sql
UPDATE users
SET phone_number = 9898180180
WHERE id = 1;
```

**Why we use it:**
Profile updates.

---

### Update Multiple

```sql
UPDATE users
SET age = 25;
```

**Why we use it:**
Bulk corrections.

---

### Update with Condition

```sql
UPDATE users
SET phone_number = 8431185195
WHERE age = 25;
```

---

### Update NULL Values

```sql
UPDATE users
SET phone_number = 8431185185
WHERE phone_number IS NULL;
```

---

## 🟢 Delete Records

### Delete One

```sql
DELETE FROM users WHERE id = 1;
```

---

### Delete All

```sql
DELETE FROM users;
```

---

## 🟢 Operators (SQL Version)

### NOT EQUAL

```sql
SELECT * FROM users WHERE name <> 'Sahil';
```

---

### BETWEEN

```sql
SELECT * FROM users WHERE age BETWEEN 23 AND 25;
```

---

### IN

```sql
SELECT * FROM users WHERE age IN (21,25,26);
```

---

### NOT IN

```sql
SELECT * FROM users WHERE age NOT IN (21,25,26);
```

---

### AND

```sql
SELECT * FROM users
WHERE age >= 25 AND name <> 'Sahil';
```

---

### OR

```sql
SELECT * FROM users
WHERE age > 20 OR name = 'Sahil';
```

---

### NOT

```sql
SELECT * FROM users WHERE NOT age >= 30;
```

---

## 🟢 Indexes (Very Important)

### Create Index

```sql
CREATE INDEX idx_users_name ON users(name);
```

**Why we use it:**
Speeds up search queries.

---

### View Indexes

```sql
\d users
```

---

### Drop Index

```sql
DROP INDEX idx_users_name;
```

---

## 🟢 Query Performance

```sql
EXPLAIN ANALYZE SELECT * FROM users WHERE name = 'Sahil';
```

**Why we use it:**
See query execution time and index usage.

---

## 🎯 Final SQL vs Mongo Teaching Line

> MongoDB gives flexibility first, structure later.
> PostgreSQL gives structure first, flexibility later.

---

