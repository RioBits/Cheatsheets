# MongoDB
Table of contents:
- <a href="#1">Importing and Exporting Data</a>
- <a href="#2">Basic Commands</a>
- <a href="#3">Find Documents</a>
- <a href="#4">Insert Documents</a>
- <a href="#5">Updating Documents</a>
- <a href="#6">Deleting Documents</a>
- <a href="#7">Query Operators</a>
  - <a href="#7-2">Comparison Operators</a>
  - <a href="#7-3">Logic Operators</a>
  - <a href="#7-4">Expressive Query Operator</a>
  - <a href="#7-5">Array Operators</a>
- <a href="#8">Projection</a>
- <a href="#9">Sub-Documents</a>
- <a href="#10">Aggregation Framework</a>
- <a href="#11">sort(), limit() and skip()</a>
- <a href="#12">Indexes</a>
- <a href="#13">Upsert Document</a>

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

Remove database
```
db.dropDatabase()
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

Find document using element inside array
```bash
db.<collection>.find({"arrayName": "element"}).pretty()
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

<h2 id="7">Query Operators</h2>

<h3 id="7-1">Update operators</h3>

- $inc
- $set
- $push

```
{<operator>: { <field>: <value> }}
```

<a href="#5">see examples</a>

<h3 id="7-2">Comparison Operators</h3>

- $eq: Equal to
- $ne: Not Equal to
- $gt: Greater Than
- $gte: Greater Than or Equal
- $lt: Less Than
- $lte: Less Than or Equal

```
{<field>: { <operator>: <value> }}
```

Examples from MongoDB University

Find all documents where the tripduration was less than or equal to 70 seconds and the usertype was not Subscriber:
```bash
db.trips.find({ "tripduration": { "$lte" : 70 },
                "usertype": { "$ne": "Subscriber" } }).pretty()
```

Find all documents where the tripduration was less than or equal to 70 seconds and the usertype was Customer using a redundant equality operator:
```bash
db.trips.find({ "tripduration": { "$lte" : 70 },
                "usertype": { "$eq": "Customer" }}).pretty()
```

Find all documents where the tripduration was less than or equal to 70 seconds and the usertype was Customer using the implicit equality operator:
```bash
db.trips.find({ "tripduration": { "$lte" : 70 },
                "usertype": "Customer" }).pretty()
```

<h3 id="7-3">Logic Operators</h3>
- $and: Match all the specified query clauses
- $or: At least one of the query clauses is matched
- $nor: Fail to match both given clauses
- $not: Negates the query requirement

`$and`, `$or` and `$nor` have similar syntax:
```
{<operator>: [{statement1}, {statement2}, ...]}
```

`$not`:
```
{$not: {statement}}
```

Find all documents where airplanes CR2 or A81 left or landed in the KZN airport:
```bash
db.routes.find({ "$and": [
  {
    "$or" :[
      {"dst_airport": "KZN" },
      { "src_airport": "KZN" }
    ]
  },
  {
    "$or" :[
      { "airplane": "CR2" },
      { "airplane": "A81" }
    ]
  }
]}).pretty()
```

<h3 id="7-4">Expressive Query Operator</h3>

$exper: allows the use of aggregation experssions within the query language
```
$exper: { <experssion> }
```

$exper: allows us to use variables and conditional statements

Find all documents where the trip started and ended at the same station:
```bash
db.trips.find({ "$expr": { "$eq": [ "$end station id", "$start station id"] }
              }).count()
```

Find all documents where the trip lasted longer than 1200 seconds, and started and ended at the same station:
```bash
db.trips.find({
  "$expr": {
    "$and": [
      { "$gt": [ "$tripduration", 1200 ]},
      { "$eq": [ "$end station id", "$start station id" ]}
    ]
  }
}).count()
```
> Watch [this video](https://www.youtube.com/watch?v=1UaDcBBSiVI) for more detailed explaination

<h3 id="7-5">Array Operators</h3>

- $push: allows us to add an element to an array, turns a field into an array field if it was previously a different type
- $size: returns a cursor with all documents where the specified field is exactly the given length.
- $all: returns a cursor with all documents in which the specified array field contains all the given elements regardless of their order in the array.

```
{"$push": {<array field>: <value>}}
{<array field>: {"$size": <number>}}
{<array field>: {"$all": <array>}}
```

Find all documents with exactly 20 amenities which include all the amenities listed in the query array:
```bash
db.listingsAndReviews.find({ "amenities": {
                                  "$size": 20,
                                  "$all": [ "Internet", "Wifi",  "Kitchen",
                                           "Heating", "Family/kid friendly",
                                           "Washer", "Dryer", "Essentials",
                                           "Shampoo", "Hangers",
                                           "Hair dryer", "Iron",
                                           "Laptop friendly workspace" ]
                                         }
                            }).pretty()
```

<h2 id="8">Projection</h2>

```
db.collection.find({ <query> }, { <projection> })
```

1 - include the field
0 - exclude the field

Ignore all expect 1s

```
db.collection.find({ <query> }, { <field1>: 1, <field2>: 1})
```

Ignore 0s

```
db.collection.find({ <query> }, { <field1>: 0, <field2>: 0})
```

You can't use both expect if you want to exclude _id from the results

```
db.collection.find({ <query> }, { <field1>: 1, "_id": 0})
```

Find all documents that have Wifi as one of the amenities only include price and address in the resulting cursor:
```bash
db.listingsAndReviews.find({ "amenities": "Wifi" },
                           { "price": 1, "address": 1, "_id": 0 }).pretty()
```

$elemMatch
```
{<field>: { "$elemMatch": { <field>: <value> } }}
```

`<field>` should contain an array field name which has object elements inside

Matches doucuments that contain an array field with at least one element that matches the specified query criteria.

**or**

Projects only the array elements with at least one element that matches the specified cirteria.

Find all documents where the student in class 431 received a grade higher than 85 for any type of assignment:
```bash
db.grades.find({ "class_id": 431 },
               { "scores": { "$elemMatch": { "score": { "$gt": 85 } } }
             }).pretty()
```

Find all documents where the student had an extra credit score:
```bash
db.grades.find({ "scores": { "$elemMatch": { "type": "extra credit" } }
               }).pretty()
```

<h2 id="9">Sub-Documents</h2>

```bash
db.<collection>.find({"<field>.<sub/array index>. ..": "<query>"})
```

```bash
db.trips.findOne({ "start station location.type": "Point" })
```

```bash
db.companies.find({ "relationships.0.person.last_name": "Zuckerberg" }).pretty()
```

<h2 id="10">Aggregation Framework</h2>

```bash
db.listingsAndReviews.aggregate([
                                  { "$match": { "amenities": "Wifi" } },
                                  { "$project": { "price": 1,
                                                  "address": 1,
                                                  "_id": 0 }}]).pretty()
```

is the same as
```bash
db.listingsAndReviews.find({ "amenities": "Wifi" },
                           { "price": 1, "address": 1, "_id": 0 }).pretty()
```

Project only the address field value for each document, then group all documents into one document per `address.country` value.
```bash
db.listingsAndReviews.aggregate([ { "$project": { "address": 1, "_id": 0 }},
                                  { "$group": { "_id": "$address.country" }}])
```

Project only the address field value for each document, then group all documents into one document per address.country value, and count one for each document in each group.
```bash
db.listingsAndReviews.aggregate([
                                  { "$project": { "address": 1, "_id": 0 }},
                                  { "$group": { "_id": "$address.country",
                                                "count": { "$sum": 1 } } }
                                ])
```

<h2 id="11">sort(), limit() and skip()</h2>

From lowest to highest
```bash
db.<collection>.find().sort({ "<field>": 1 })
```

No effect
```bash
db.<collection>.find().sort({ "<field>": 0 })
```

From highest to lowest
```bash
db.<collection>.find().sort({ "<field>": -1 })
```

Get only 20 result
```bash
db.<collection>.find({ "<field>": "<query>" }).limit(20)
```

Skip the first 20 result
```bash
db.<collection>.find({ "<field>": "<query>" }).skip(20)
```

<h2 id="12">Indexes</h2>

Used for better performance
see [this video](https://www.youtube.com/watch?v=vl6htnqI4Kw)

<h2 id="13">Upsert Document</h2>

Used when you want to replace document or create document if it's not exist
```bash
db.<collection>.updateOne({"<field>": "<query>"}, {"<update>"}, {"upsert": true})
```
For detailed explaination see [this video](https://youtu.be/-56x56UppqQ?t=960)

sources: [https://university.mongodb.com](https://university.mongodb.com)
