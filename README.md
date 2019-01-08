# Mongoose-with-MongoDB
Use Mongoose to manipulate a MongoDB database

## Objectives:
- Explain what Mongoose is.
- Use Mongoose to interact with MongoDB in order to create a full CRUD App. 
- Differentiate between NoSQL and SQL databases.
- Describe the role of Mongoose schema and models.
- Understand Mongoose queries.
- Persist data using Mongoose embedded documents.
- Describe how to use validations in Mongoose.

<br />


#### What is a NoSQL database?

A NoSQL database is a non-relational database.

* NoSQL databases are purpose built for specific data models and have flexible schemas for building modern applications. NoSQL databases are widely recognized for their ease of development, functionality, and performance at scale. They use a variety of data models, including document, graph, key-value, in-memory, and search.

* There is not an explicit one-to-one, one-to-many and many-to-many relationship.

#### What attributes are used in a NOSQL database?

NoSQL database's use **documents** and **collections** to organize data.

* Think of Collections like a large box that holds several items.
* Think of Documents like the items within that large box.

#### What is MongoDB?

MongoDB is a NoSQL database that stores information as JSON.
BSON is a computer data interchange format used mainly as a data storage and network transfer format in the MongoDB database.

#### Basic MongoDB commands?
There are several MongoDB commands, we will use only a few commands to test what is in our database. Such as:

* `show dbs` - shows a list of all databases
* `use database-name` - connects to a database
* `show collections` - lists the collections that exist within a database
* `db.students.find()` - lists all of the students in the student collection

Check out this [link](https://docs.mongodb.com/manual/reference/command/) to learn more. 

## MongoDB Database Structure

![](https://i.imgur.com/ZAQOhhY.png)
