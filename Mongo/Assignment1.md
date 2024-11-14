Let's set up an example MongoDB database with a collection of documents to demonstrate the following operations: inserting, updating, removing documents, and executing various queries. We'll use a simple `students` database with a `grades` collection to store information about student names, grades, subjects, and other attributes.

### Step 1: Setting Up MongoDB and Creating the Database

1. **Create the database and collection**:
   ```javascript
   use students;
   db.createCollection("grades");
   ```

2. **Insert Sample Data**:
   Here’s a batch insert of documents with validation to ensure each document contains required fields.

   ```javascript
   db.grades.insertMany([
      { name: "Alice", subject: "Math", grade: 85, age: 20, hobbies: ["reading", "chess"] },
      { name: "Bob", subject: "Science", grade: 90, age: 22, hobbies: ["music", "sports"] },
      { name: "Charlie", subject: "Math", grade: 70, age: 21, hobbies: ["chess", "coding"] },
      { name: "Daisy", subject: "English", grade: 95, age: 20, hobbies: ["dancing", "music"] },
      { name: "Eve", subject: "Science", grade: 88, age: 22, hobbies: ["writing", "reading"] },
   ]);
   ```

3. **Insert Validation**:
   Insert a single document with validation by checking fields before insertion.

   ```javascript
   const doc = { name: "Frank", subject: "History", grade: 92, age: 23 };
   if (doc.name && doc.subject && doc.grade) {
      db.grades.insertOne(doc);
   } else {
      print("Document missing required fields");
   }
   ```

### Step 2: Removing Documents

- **Remove a document**:
  ```javascript
  db.grades.deleteOne({ name: "Alice" });
  ```

- **Remove multiple documents**:
  ```javascript
  db.grades.deleteMany({ subject: "Science" });
  ```

### Step 3: Updating Documents

1. **Replace a document**:
   ```javascript
   db.grades.replaceOne(
      { name: "Bob" },
      { name: "Bob", subject: "Physics", grade: 95, age: 22, hobbies: ["guitar", "running"] }
   );
   ```

2. **Update using modifiers**:
   - **Increment grade by 5**:
     ```javascript
     db.grades.updateOne({ name: "Charlie" }, { $inc: { grade: 5 } });
     ```

   - **Add a new hobby**:
     ```javascript
     db.grades.updateOne({ name: "Charlie" }, { $push: { hobbies: "gaming" } });
     ```

3. **Upsert**:
   If a student named "Grace" doesn’t exist, this will insert her document; otherwise, it will update her age.

   ```javascript
   db.grades.updateOne(
      { name: "Grace" },
      { $set: { age: 24, subject: "Math", grade: 89 } },
      { upsert: true }
   );
   ```

4. **Update multiple documents**:
   Update the grades of all students who study "Math."

   ```javascript
   db.grades.updateMany({ subject: "Math" }, { $set: { grade: 80 } });
   ```

5. **Return the updated document**:
   Using `findOneAndUpdate` to retrieve and update a document.

   ```javascript
   db.grades.findOneAndUpdate(
      { name: "Daisy" },
      { $set: { grade: 97 } },
      { returnNewDocument: true }
   );
   ```

### Step 4: Executing MongoDB Queries

Here are 10 sample queries demonstrating different aspects of MongoDB querying.

1. **Find and Find One (specific values)**:
   - Find all documents where the subject is "Math."
     ```javascript
     db.grades.find({ subject: "Math" });
     ```

   - Find one document with a specific name.
     ```javascript
     db.grades.findOne({ name: "Charlie" });
     ```

2. **Query Criteria**:
   - Find students with a grade greater than 85.
     ```javascript
     db.grades.find({ grade: { $gt: 85 } });
     ```

   - OR query: Find students who are either in Science or Math.
     ```javascript
     db.grades.find({ $or: [{ subject: "Science" }, { subject: "Math" }] });
     ```

   - `$not` Query: Find students who do not have "dancing" as a hobby.
     ```javascript
     db.grades.find({ hobbies: { $not: { $eq: "dancing" } } });
     ```

   - Conditional Semantics: Find students older than 21 and with grade over 80.
     ```javascript
     db.grades.find({ age: { $gt: 21 }, grade: { $gt: 80 } });
     ```

3. **Type-Specific Queries**:
   - Find students with null in any field.
     ```javascript
     db.grades.find({ grade: null });
     ```

   - Regular Expression: Find students whose names start with "A."
     ```javascript
     db.grades.find({ name: /^A/ });
     ```

   - Querying Arrays: Find students who have "reading" as one of their hobbies.
     ```javascript
     db.grades.find({ hobbies: "reading" });
     ```

4. **$where Queries**:
   Using `$where` to perform a more complex query, like finding students with grades greater than their age.

   ```javascript
   db.grades.find({ $where: "this.grade > this.age" });
   ```

5. **Cursors (Limit, Skip, Sort, Advanced Query Operations)**:
   - Sort the documents by grade in descending order.
     ```javascript
     db.grades.find().sort({ grade: -1 });
     ```

   - Limit the results to only 3 documents.
     ```javascript
     db.grades.find().limit(3);
     ```

   - Skip the first 2 documents and then retrieve the next set.
     ```javascript
     db.grades.find().skip(2).limit(3);
     ```

### Summary

This example covers essential MongoDB operations:
- Inserting, batch inserting, and inserting with validation
- Removing single and multiple documents
- Updating documents in multiple ways: replacement, using modifiers, upserts, and multi-document updates
- Queries covering common MongoDB functionality, including `find`, query operators, and cursors
