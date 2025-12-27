

## 🔁 SQL ↔ MongoDB Command Mapping (Most Important Table)

![Image](https://substackcdn.com/image/fetch/%24s_%213Es9%21%2Cf_auto%2Cq_auto%3Agood%2Cfl_progressive%3Asteep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa81088b7-a188-4089-845a-63936e930a71_1632x1076.png)

![Image](https://www.datensen.com/blog/wp-content/uploads/postgresql-vs-mongodb.png)

### 📌 Database & Collection / Table

| SQL (PostgreSQL)      | MongoDB                        | Why                      |
| --------------------- | ------------------------------ | ------------------------ |
| `CREATE DATABASE db;` | `use db`                       | Create / switch database |
| `DROP DATABASE db;`   | `db.dropDatabase()`            | Delete database          |
| `CREATE TABLE users`  | `db.createCollection("users")` | Structure container      |
| `DROP TABLE users;`   | `db.users.drop()`              | Delete data structure    |

---

### 📌 Insert Data

| SQL                              | MongoDB        | Why                  |
| -------------------------------- | -------------- | -------------------- |
| `INSERT INTO users VALUES (...)` | `insertOne()`  | Insert single record |
| `INSERT INTO users VALUES (),()` | `insertMany()` | Bulk insert          |

---

### 📌 Read Data

| SQL                              | MongoDB                      | Why            |
| -------------------------------- | ---------------------------- | -------------- |
| `SELECT * FROM users`            | `find()`                     | Fetch all data |
| `SELECT * FROM users WHERE id=1` | `findOne({_id})`             | Single record  |
| `SELECT name,phone FROM users`   | `find({}, {name:1,phone:1})` | Projection     |

---

### 📌 Filtering

| SQL                           | MongoDB                           |
| ----------------------------- | --------------------------------- |
| `WHERE age > 25`              | `{ age: { $gt: 25 } }`            |
| `WHERE age BETWEEN 23 AND 25` | `{ age: { $gte: 23, $lte: 25 } }` |
| `WHERE age IN (21,25)`        | `{ age: { $in: [21,25] } }`       |
| `WHERE age NOT IN (21,25)`    | `{ age: { $nin: [21,25] } }`      |
| `WHERE name <> 'Sahil'`       | `{ name: { $ne: 'Sahil' } }`      |

---

### 📌 Logical Operators

| SQL                   | MongoDB |
| --------------------- | ------- |
| `AND`                 | `$and`  |
| `OR`                  | `$or`   |
| `NOT`                 | `$not`  |
| ❌ (no direct keyword) | `$nor`  |

---

### 📌 Sorting & Limiting

| SQL                  | MongoDB           |
| -------------------- | ----------------- |
| `ORDER BY name DESC` | `sort({name:-1})` |
| `LIMIT 3`            | `limit(3)`        |

---

### 📌 Update

| SQL                           | MongoDB                           |
| ----------------------------- | --------------------------------- |
| `UPDATE users SET age=25`     | `updateMany({}, {$set:{age:25}})` |
| `UPDATE users SET phone=NULL` | `$unset`                          |
| `WHERE phone IS NULL`         | `{ phone: { $exists:false } }`    |

---

### 📌 Delete

| SQL                            | MongoDB          |
| ------------------------------ | ---------------- |
| `DELETE FROM users WHERE id=1` | `deleteOne()`    |
| `DELETE FROM users`            | `deleteMany({})` |

---

### 📌 Index & Performance

| SQL               | MongoDB                     |
| ----------------- | --------------------------- |
| `CREATE INDEX`    | `createIndex()`             |
| `EXPLAIN ANALYZE` | `explain("executionStats")` |

---

## 🧠 Interview Questions with Crisp Answers

### 1️⃣ SQL vs NoSQL – Core

**Q:** What is the main difference between SQL and NoSQL?
**A:**
SQL uses a **fixed schema and relational tables**, while NoSQL uses **flexible schema and document-based storage**, optimized for scalability.

---

### 2️⃣ Why MongoDB over SQL?

**Q:** When would you choose MongoDB instead of PostgreSQL?
**A:**
When the application needs **schema flexibility**, **high write throughput**, and **horizontal scaling** (e.g., social media, logs).

---

### 3️⃣ Why SQL still exists?

**Q:** Why do banks prefer SQL databases?
**A:**
Because SQL provides **ACID transactions**, **data integrity**, and **complex joins**, which are critical for financial systems.

---

### 4️⃣ What is Projection?

**Q:** What is projection in MongoDB?
**A:**
Selecting only required fields from documents to **reduce payload and improve performance**.

---

### 5️⃣ `updateOne` vs `updateMany`

**Q:** Difference between `updateOne()` and `updateMany()`?
**A:**
`updateOne()` updates the **first matched document**, `updateMany()` updates **all matching documents**.

---

### 6️⃣ `$set` vs `$unset`

**Q:** Difference between `$set` and `$unset`?
**A:**
`$set` adds or updates a field, `$unset` **removes the field completely**.

---

### 7️⃣ `$exists`

**Q:** Why do we use `$exists`?
**A:**
To find or fix documents where a field is **missing**, common in old data.

---

### 8️⃣ Indexes

**Q:** Why are indexes important?
**A:**
Indexes **reduce the number of documents scanned**, improving query speed drastically.

---

### 9️⃣ Explain / Execution Plan

**Q:** Why use `explain()` or `EXPLAIN ANALYZE`?
**A:**
To understand **query performance**, index usage, and optimization opportunities.

---

### 🔟 Empty Filter `{}` Danger

**Q:** What happens if we pass `{}` in update or delete?
**A:**
It affects **ALL documents** — very dangerous in production.

---

### 1️⃣1️⃣ Schema-less Myth

**Q:** Is MongoDB completely schema-less?
**A:**
No. It is **schema-flexible**, but schema design is still required for production.

---

### 1️⃣2️⃣ Capped Collections

**Q:** What are capped collections used for?
**A:**
For **fixed-size, FIFO data** like logs and events.


