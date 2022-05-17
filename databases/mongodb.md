# MongoDB
Table of contents:
- <a href="#1">Importing and Exporting Data</a>
- <a href="#2">Basic Commands</a>
- <a href="#3">Find Documents</a>
- <a href="#4">Insert Documents</a>
- <a href="#5">Updating Documents</a>
- <a href="#6">Deleting Documents</a>

MongoDB is NoSQL database

NoSQL databases (aka "not only SQL") are non-tabular databases and store data differently than relational tables. 

Data in MongoDB is stored as documents

Some terms on MongoDB:

Cluster:
collection of datasets distributed across many shards (servers) in order to achieve horizontal scalability and better performance in read and write operations.

![Example](https://webimages.mongodb.com/_com_assets/cms/kses6yxn96rf33qkk-mongodb-sharded-cluster.png?auto=format%252Ccompress)

Collection:
grouping of MongoDB documents

![Example](https://www.mongodb.com/docs/manual/images/crud-annotated-collection.bakedsvg.svg)

Document:
document database designed for ease of application development and scaling

Example:
```json
{
  "name": "al",
  "age": 18,
  "status": "D",
  "groups": ["politics", "news"]
}
```

MongoDB stores the data as BSON (Binary Javascript Object Notation)

<h2 id="1">Importing and Exporting Data</h2>

JSON:
- mongoimport
  ```bash
  mongoimport --uri="mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies" --drop sales.json
  ```
  `--drop`: drops the collection
  `file.json`: the file that you want to import to the database
- mongoexport
  ```bash
  mongoexport --uri="mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies" --collection=sales --out=sales.json
  ```
  `--collection`: collection name
  `--out`: output file

BSON:
- mongorestore
  ```bash
  mongorestore --uri "mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies"  --drop dump
  ```
  `--drop`: drops the collection
  `dump`: folder name
- mongodump
  ```bash
  mongodump --uri "mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies"
  ```

URI (Uniform Resource Identifier)
looks like:
```
mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/<default database>
```
`srv`: establishes a secure connection 

<h2 id="2">Basic Commands</h2>

Show Databases
```bash
show dbs
```

Use/Create database
```
use <database name>
```

Show collections
```
show collections
```

<h2 id="3">Find Documents</h2>

Get one document
```bash
db.<collection>.findOne()
```

> replace `<collection>` with the collection name


Get all documents
```bash
db.<collection>.find()
```

Get specific documents
```bash
db.<collection>.find({"state": "NY"})
```

Number of All
```bash
db.<collection>.find().count()
```

Number of specific documents
```bash
db.<collection>.find({"state": "NY"}).count()
```

Get data with better shape
```bash
db.<collection>.find().pretty()
```

If you got a lot of data you can use this command to get the next page (Iterates through a cursor)
```bash
it
```

<h2 id="4">Insert Documents</h2>

Each document has unique required `_id`
ObjectId() is default value for the `_id` field unless otherwise specified.

You can't insert a document with the same id, so if you want to insert a new one make sure to delete the one that has the same id

Insert one Document
```bash
db.<collection>.insertOne({
  "certificate_number" : 9278806,
  "business_name" : "ATLIXCO DELI GROCERY INC.",
  "date" : "Feb 20 2015",
  "result" : "No Violation Issued",
  "sector" : "Cigarette Retail Dealer - 127",
  "address" : {
    "city" : "RIDGEWOOD",
    "zip" : 11385,
    "street" : "MENAHAN ST",
    "number" : 1712
  }
})
```

Insert more than one document
```bash
db.<collection>.insertMany([ { "test": 1 }, { "test": 2 }, { "test": 3 } ])
```

Insert documents without order
```bash
db.<collection>.insert([{ "_id": 1, "test": 1 },{ "_id": 1, "test": 2 },
                       { "_id": 3, "test": 3 }],{ "ordered": false })
```

> Watch [this video](https://www.youtube.com/watch?v=ovE0c3_khMA) for more detailed explaination

<h2 id="5">Updating Documents</h2>

Update operators:
- $inc
- $set
- $push

Increments field value by a specified amount.
```
{"$inc": {"pop": 10, "<field2>": "<increment value>", ...}}
```

Sets field value to a new specified value.
```
{"$set": {"pop": 17630, "<field2>": "<increment value>", ...}}
```

Adds an element to an array field or creates a new array field with element.
```
{"$push": {"<field1>": "<value1>", ...}}
```

Update Document
```bash
db.<collection>.updateOne(<queries>, <updates>)
```
```bash
db.<collection>.updateOne({ "zip": "12534" }, { "$set": { "pop": 17630 } })
```

Adds new field

```bash
db.zips.updateOne({ "zip": "12534" }, { "$set": { "population": 17630 } })
```

Update Documents
```bash
db.<collection>.updateMany({ "city": "HUDSON" }, { "$inc": { "pop": 10 } })
```

```bash
db.grades.updateOne({ "student_id": 250, "class_id": 339 }, {
  "$push": {
    "scores": {
      "type": "extra credit",
      "score": 100
    }
  }
})
```

<h2 id="6">Deleting Documents</h2>

Delete one document
```bash
db.<collection>.deleteOne({ "username": "rio" })
```

Delete more than one document
```bash
db.<collection>.deleteMany({ "category": "developer" })
```

Delete collection and its documents
```bash
db.<collection>.drop()
```

sources: [https://university.mongodb.com](https://university.mongodb.com)