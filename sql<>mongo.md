

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

Below is a **beginner-friendly, interview-ready list** of **SQL + MongoDB questions**.
Each question has a **short, crisp answer** and mixes **conceptual + programmatic** questions — exactly how entry-level interviews are structured.

---

# 🧠 SQL Interview Questions (Beginners)

![Image](https://s33046.pcdn.co/wp-content/uploads/2018/12/word-image-228.png)

![Image](https://miro.medium.com/1%2Awr_PNTP9fQHxXeMydaSfnQ.jpeg)

---

## 🔹 Conceptual (Short Answers)

### 1️⃣ What is SQL?

**Ans:**
SQL (Structured Query Language) is used to **store, retrieve, and manipulate data** in relational databases.

---

### 2️⃣ What is a table?

**Ans:**
A table stores data in **rows and columns** with a fixed schema.

---

### 3️⃣ What is a primary key?

**Ans:**
A column that **uniquely identifies each row** in a table.

---

### 4️⃣ What is NULL?

**Ans:**
Represents **missing or unknown data**, not zero or empty string.

---

### 5️⃣ What is a foreign key?

**Ans:**
A column that creates a **relationship between two tables**.

---

### 6️⃣ What is normalization?

**Ans:**
Process of **reducing data redundancy** by splitting tables.

---

### 7️⃣ What are indexes?

**Ans:**
Special data structures that **speed up SELECT queries**.

---

### 8️⃣ What is ACID?

**Ans:**
Atomicity, Consistency, Isolation, Durability — ensures **reliable transactions**.

---

## 🔹 Programmatic (With Short Answers)

### 9️⃣ How to select all rows?

```sql
SELECT * FROM users;
```

---

### 🔟 How to filter rows?

```sql
SELECT * FROM users WHERE age > 25;
```

---

### 1️⃣1️⃣ How to insert data?

```sql
INSERT INTO users (name, age) VALUES ('Sahil', 25);
```

---

### 1️⃣2️⃣ How to update a record?

```sql
UPDATE users SET age = 26 WHERE id = 1;
```

---

### 1️⃣3️⃣ How to delete a record?

```sql
DELETE FROM users WHERE id = 1;
```

---

### 1️⃣4️⃣ Difference between DELETE and TRUNCATE?

**Ans:**
DELETE removes rows **one by one**, TRUNCATE removes **all rows instantly**.

---

### 1️⃣5️⃣ How to sort data?

```sql
SELECT * FROM users ORDER BY name DESC;
```

---

### 1️⃣6️⃣ How to limit results?

```sql
SELECT * FROM users LIMIT 5;
```

---

### 1️⃣7️⃣ What is JOIN?

**Ans:**
Used to **combine rows from multiple tables**.

---

# 🧠 MongoDB Interview Questions (Beginners)

![Image](https://cdn.prod.website-files.com/68ac1d7405234ac5768d8914/68cbc26ff47829cb2e2d4a46_screenshot-2023-08-28-at-3-31-52-pm.png)

![Image](https://webimages.mongodb.com/_com_assets/cms/lyg8ziob3mi9rb6i3-image1.png?auto=format%252Ccompress)

---

## 🔹 Conceptual (Short Answers)

### 1️⃣ What is MongoDB?

**Ans:**
MongoDB is a **NoSQL document-based database** that stores data in JSON-like format.

---

### 2️⃣ What is a collection?

**Ans:**
A collection is a group of **documents**, similar to a table in SQL.

---

### 3️⃣ What is a document?

**Ans:**
A document is a **single record** stored as a JSON-like object.

---

### 4️⃣ What is `_id`?

**Ans:**
A **unique identifier** automatically generated for each document.

---

### 5️⃣ Is MongoDB schema-less?

**Ans:**
No. It is **schema-flexible**, not schema-less.

---

### 6️⃣ What are indexes?

**Ans:**
Indexes **speed up query performance** by reducing scanned documents.

---

### 7️⃣ What is BSON?

**Ans:**
Binary JSON format used internally by MongoDB.

---

### 8️⃣ What is a capped collection?

**Ans:**
A fixed-size collection that follows **FIFO** behavior.

---

## 🔹 Programmatic (With Short Answers)

### 9️⃣ How to insert one document?

```js
db.users.insertOne({ name: "Sahil", age: 25 });
```

---

### 🔟 How to insert multiple documents?

```js
db.users.insertMany([{ name: "Amit" }, { name: "Neha" }]);
```

---

### 1️⃣1️⃣ How to find all documents?

```js
db.users.find();
```

---

### 1️⃣2️⃣ How to find one document?

```js
db.users.findOne({ name: "Sahil" });
```

---

### 1️⃣3️⃣ How to filter documents?

```js
db.users.find({ age: { $gt: 25 } });
```

---

### 1️⃣4️⃣ How to update a document?

```js
db.users.updateOne(
  { name: "Sahil" },
  { $set: { age: 26 } }
);
```

---

### 1️⃣5️⃣ How to delete a document?

```js
db.users.deleteOne({ name: "Sahil" });
```

---

### 1️⃣6️⃣ What does `$set` do?

**Ans:**
Adds or updates a field in a document.

---

### 1️⃣7️⃣ What does `$unset` do?

**Ans:**
Removes a field from a document.

---

### 1️⃣8️⃣ What is `$exists`?

**Ans:**
Checks whether a field exists in a document.

---

### 1️⃣9️⃣ What is projection?

```js
db.users.find({}, { name: 1, _id: 0 });
```

**Ans:**
Returns only selected fields.

---

### 2️⃣0️⃣ What happens if filter is `{}`?

**Ans:**
The operation affects **all documents** — dangerous in production.

---

## 🔁 SQL vs MongoDB (Quick Interview Round)

| Question                      | Answer                                        |
| ----------------------------- | --------------------------------------------- |
| SQL table vs Mongo collection | Table is fixed schema, collection is flexible |
| Row vs Document               | Row is structured, document is JSON-like      |
| JOIN vs Embed                 | JOIN combines tables, embed nests data        |
| Scaling                       | SQL → vertical, Mongo → horizontal            |

---



