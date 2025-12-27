
# 🟢 Step 1: Basic MongoDB Startup Commands

### 1️⃣ `show databases`

**What it does:**
Displays all existing databases on the MongoDB server.

**Why we use it:**
To verify which databases already exist (dev, test, prod).

```js
show databases
```

---

### 2️⃣ `db.createCollection()`

**What it does:**
Creates a new empty collection inside the selected database.

**Why we use it:**
Collections store documents (similar to tables in SQL).

```js
db.createCollection("users")
```

---

### 3️⃣ `db.dropDatabase()`

**What it does:**
Deletes the currently selected database completely.

**Why we use it:**
To reset data during development or testing.

```js
db.dropDatabase()
```

---

### 4️⃣ `insertOne()`

**What it does:**
Inserts a single document into a collection.

**Why we use it:**
To add one record (example: single user signup).

```js
db.users.insertOne({ name: "Sahil" })
```

---

# 🟢 Step 2: ONE `insertMany()` to Set Up Everything

> This **single command prepares data** for **ALL remaining queries**

```js
db.users.insertMany([
  {
    name: "Sahil",
    age: 25,
    phoneNumber: 8431185185,
    salary: 55000.75,
    isActive: true,
    skills: ["React", "Node"],
    createdAt: new Date(),
    address: { city: "Hyderabad", pincode: 500081 }
  },
  {
    name: "Amit",
    age: 23,
    phoneNumber: 8431185195,
    salary: 42000,
    isActive: false,
    skills: ["Java"],
    createdAt: new Date("2024-01-10"),
    address: { city: "Bangalore", pincode: 560001 }
  },
  {
    name: "Rohit",
    age: 21,
    salary: 38000,
    isActive: true,
    skills: ["Python"],
    createdAt: new Date("2023-11-15"),
    address: { city: "Pune", pincode: 411001 }
  },
  {
    name: "Neha",
    age: 26,
    phoneNumber: 9898180180,
    salary: null,
    isActive: true,
    skills: [],
    createdAt: new Date(),
    address: { city: "Mumbai", pincode: 400001 }
  }
]);
```

---

# 🟢 Step 3: Commands + 1–2 Line Theory + Why We Use Them

---

### `find()`

**What it does:**
Fetches all documents from a collection.

**Why we use it:**
To view stored data or debug inserts.

```js
db.users.find()
```

---

### MongoDB Data Types

**What it shows:**
MongoDB supports flexible data types per field.

**Why we use it:**
Allows schema flexibility (unlike SQL).

✔ string → `name`
✔ integer → `age`
✔ double → `salary`
✔ boolean → `isActive`
✔ array → `skills`
✔ date → `createdAt`
✔ null → `salary`
✔ object → `address`

---

### `sort()`

**What it does:**
Sorts documents in ascending or descending order.

**Why we use it:**
To display ordered results (latest, alphabetical, etc.).

```js
db.users.find().sort({ name: -1 })
```

---

### `limit()`

**What it does:**
Restricts number of documents returned.

**Why we use it:**
Pagination and performance optimization.

```js
db.users.find().limit(3)
```

---

### Filter by Multiple Fields

**What it does:**
Fetches documents matching all conditions.

**Why we use it:**
Precise searches (login, profile lookup).

```js
db.users.find({ name: "Sahil", phoneNumber: 8431185185 })
```

---

### Projection

**What it does:**
Returns only selected fields.

**Why we use it:**
Reduce payload size & improve performance.

```js
db.users.find({}, { _id: false, name: true, phoneNumber: true })
```

---

### `updateOne()`

**What it does:**
Updates a single matching document.

**Why we use it:**
Profile updates, phone/email change.

```js
db.users.updateOne(
  { _id: ObjectId("id") },
  { $set: { phoneNumber: 9898180180 } }
)
```

---

### `findOne()`

**What it does:**
Returns the first matching document.

**Why we use it:**
Fetch a single record (profile page).

```js
db.users.findOne({ _id: ObjectId("id") })
```

---

### `$unset`

**What it does:**
Removes a field from a document.

**Why we use it:**
Delete optional or obsolete fields.

```js
db.users.updateOne(
  { _id: ObjectId("id") },
  { $unset: { phoneNumber: "" } }
)
```

---

### `updateMany()` (All Documents)

**What it does:**
Updates every document due to empty filter `{}`.

**Why we use it:**
Bulk corrections or migrations.

```js
db.users.updateMany({}, { $set: { age: 25 } })
```

---

### Update Based on Condition

```js
db.users.updateMany(
  { age: 25 },
  { $set: { phoneNumber: 8431185195 } }
)
```

**Why we use it:**
Targeted bulk updates.

---

### `$exists`

```js
db.users.updateMany(
  { phoneNumber: { $exists: false } },
  { $set: { phoneNumber: 8431185185 } }
)
```

**Why we use it:**
Fix missing fields in old data.

---

### `deleteOne()`

**What it does:**
Deletes one document.

**Why we use it:**
Remove a single user.

```js
db.users.deleteOne({ _id: ObjectId("id") })
```

---

### `deleteMany()`

**What it does:**
Deletes all matching documents.

**Why we use it:**
Clear test data.

```js
db.users.deleteMany({})
```

---

### `$ne` (Not Equal)

```js
db.users.find({ name: { $ne: "Sahil" } })
```

**Why we use it:**
Exclude specific values.

---

### `$gte` & `$lte`

```js
db.users.find({ age: { $gte: 23, $lte: 25 } })
```

**Why we use it:**
Range filtering (age, salary, date).

---

### `$in`

```js
db.users.find({ age: { $in: [21, 25, 26] } })
```

**Why we use it:**
Match values from a list.

---

### `$nin`

```js
db.users.find({ age: { $nin: [21, 25, 26] } })
```

**Why we use it:**
Exclude values from a list.

---

### `$and`

```js
db.users.find({
  $and: [{ age: { $gte: 25 } }, { name: { $ne: "Sahil" } }]
})
```

**Why we use it:**
Combine multiple strict conditions.

---

### `$or`

```js
db.users.find({
  $or: [{ age: { $gt: 20 } }, { name: "Sahil" }]
})
```

**Why we use it:**
Flexible condition matching.

---

### `$nor`

```js
db.users.find({
  $nor: [{ age: 25 }, { name: "Sahil" }]
})
```

**Why we use it:**
Exclude multiple conditions.

---

### `$not`

```js
db.users.find({ age: { $not: { $gte: 30 } } })
```

**Why we use it:**
Negate a condition.

---

### Create Index

```js
db.users.createIndex({ name: 1 })
```

**Why we use it:**
Improve query performance.

---

### Get Indexes

```js
db.users.getIndexes()
```

**Why we use it:**
Check existing indexes.

---

### Drop Index

```js
db.users.dropIndex("name_1")
```

**Why we use it:**
Remove unused indexes.

---

### Capped Collection

```js
db.createCollection("logs", {
  capped: true,
  size: 10000000,
  max: 100
})
```

**Why we use it:**
FIFO data like logs or events.

---

### `explain()`

```js
db.users.find().explain("executionStats")
```

**Why we use it:**
Analyze query performance.
