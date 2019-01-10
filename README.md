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

* Think of Collections like a large box that holds several items. A MongoDB database consists of collections of documents.
* Think of Documents like the items within that large box. A document in MongoDB is composed of field (key) and value pairs.


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

### Lets take a look of what a MongoDB _document_ may look like:

```js
{
    _id: ObjectId("5099803df3f4948bd2f98391"),
    name: { first: "Homer", last: "Simpson" },
    birth: new Date('Jun 23, 1912'),
    grade: "A"
    class: "Web Development"
}
```
Looks just like a JSON object!

A MongoDB _document_ is very much like JSON, except that it is stored in the database, in a format known as _BSON. BSON_ basically provides _JSON_ with additional data types, such as __ObjectID__ as shown above.

#### The Document *_id*

The *_id* is a unique field that represents the document's _primary key_ and will always be listed as the first field. SInce it is unique it cannot be duplicated.  Primary keys will become very important when we discuss embedded documents.

We can explicitly set the *_id* like this:

```js
{
  _id: 2,
  name: "Devan"
}
```
or this...

```js
{
  _id: "ABC",
  name: "Devan"
}
```
However, it is more common to allow MongoDB to create it implicitly for us, using it's _ObjectID_ data type.

<br />

## Mongoose 

![mongoose.js](https://cdn-images-1.medium.com/max/1600/1*rchG6FrxrvUsgxnfgoq8ow.png)

**Mongoose** is an ODM (Object Data Mapping), that allows us to model our data in our applications. It gives us access to additional helpers, functions and queries to simply and easily perform **CRUD** actions.

<br />

## Time to Code Along!Let's Set Up Mongoose & Create an App 
### Step 1: Set up Mongoose

1. Create a new project folder on your Desktop folder: `$ mkdir mongoose-intro-lesson`

2. `cd` into the new folder. 

3. Set up Express using the Express Generator `npm install express-generator -g`

4. Set the app name and the view engine for express `express --view=pug myapp`
- cd into myapp

5. `$ npm init -y`

6. Create a new database directory inside that folder: `$ mkdir db`

7. Create the following files: `$ touch gitignore  dotenv db/schema.js db/seeds.js` 

5. Open the app in VSCode: `$ code .`


Your folder structure should look like this:

![](https://imgur.com/v3dL2LW)

6. Install Mongoose `$ npm install mongoose`

7. Install MongoDB `brew install mongo` or `brew reinstall mongo` and then `brew services start mongo`

8. npm install dotenv

9. npm install pug

10. Go into your dotenv and add the path to the database `MONGODB_URI=mongodb://localhost/students`

11. Go into your `.gitignore` file and add `node_modules/

<br />

### Step 2:Require Mongoose & Establish connection

First, we need to require our databse in our app.js. We need to also require our dotenv file which has the path our database.  
```
require('dotenv').config();
const mongoose = require('mongoose');
mongoose.connect(process.env.MONGODB_URI); 
```
We need to require Mongoose as well as import `Schema` from Mongoose in our Schema file. 

```var mongoose = require('mongoose');
const Schema = mongoose.Schema
```
Here we have actually named our database "students". Mongo does not require that the database already exist, it will create it once try to save something. 

<br />
 ## What's a Schema?
 
 A schema is like a template for our database. It ensures that each time we save items to our database they have the same attributes and structure.
 
 ### Step 4: Set up our Student & Project Schema's
  
  Let's create an object named Project and defines it's attributes.
  
  ```
  var ProjectSchema = new Schema({
  title: String,
  unit: String
});
  ```
  and let's do the same for our Student Schema
  ```
 var StudentSchema = new Schema({
  name: String,
  gradeAverage: Number,
  projects: [ProjectSchema]
});
```
and one last thing, we need to export these schema's for later use
```
const ProjectModel = mongoose.model('Project', ProjectSchema)

const StudentModel = mongoose.model('Student', StudentSchema)
module.exports = { ProjectModel, StudentModel } 
```
In this example, we are establishing the structure of both the project and student documents within our collection. We are also establishing a one-to-many relationship between the two. Since, each student can have mulitple projects. Due to this relationship, we are embedding the Project document into the Student document as shown in the example above. 


[Embedded Documents](http://mongoosejs.com/docs/2.7.x/docs/embedded-documents.html)
<br />

Now, go into to your seeds file. Here we will populate the database. First, we need to require Mongoose and connect to our database.

This allows us to have a connection to our student database.
```
require('dotenv').config();
const Schema = mongoose.Schema
const mongoose = require('mongoose')
mongoose.connect('process.env.MONGODB_URI')
```
 Since we stored the path to our databse within our `.env` file we will also need to require it as well. We will also require Schema from Mongoose.
 
...and then we require our Student & Project Schema's
```
const {ProjectModel} = require('./schema')
const {StudentModel} = require('./schema')

```
Now let's connect!
```
mongoose.connect(process.env.MONGODB_URI)
    .then(() => {
        console.log('connected to mongoDB')
    })
    .catch((err) => {
        console.log('ERROR', err)
    })
```
This asynchronous function allows us to connect to our databse and informs us if soemthing goes wrong. A asynchronous function is a function which operates asynchronously via the event loop, using an implicit Promise to return its result. It doesn't block the calling thread while waiting for a reply. Instead, the calling thread is notified when the reply arrives.

Next up, let's insert some dummy data:
```const project1 = new ProjectModel({
    projectName: 'Project3',
    unit: 'Mongoose',
})
const project1 = new ProjectModel({
    projectName: 'Project3',
    unit: 'Mongoose',
})
const student1 = new StudentModel({
   name: 'Devan',
    gradeAverage: '87',
    projects: [project1, project2]
})
const student2 = new StudentModel({
   name: 'Valencia',
    gradeAverage: '89',
    projects: [project1, project2]
})
```

### Your Turn!

Create two more students each with two projects.

<br />

Next, let's save this data to our database.

```
const projects = [project1, project2]
const students= [student1, student2]

StudentModel.deleteMany()
    .then(() => ProjectModel.deleteMany())
     .then(() => StudenttModel.insertMany(students))
    .then(() => ProjectModel.insertMany(projects))
    .then(() => mongoose.connection.close())
```
We first removed any existing data from our database and then we inserted and saved this new data. Now run your seeds file!
` node seeds.js`
<br />

### Check out the data!

In your terminal type `mongo`. This initiates MongoDB.

If we want to see a list of all our database type `show dbs`

Once you locate your db type `use students`

To see all the documents within the students collection type `db.students.find()`

Your results should look something like this
```
 "_id" : ObjectId("5c3641a706dfefd94bf2eda7"), "name" : "Devan", "gradeAverage" : "B", "projects" : [ { "_id" : ObjectId("5c3641a706dfefd94bf2eda5"), "title" : "Project3", "unit" : "Mongoose" }
 ```
 We can add .pretty() to the end of this statement to format the data.
 

 ```
 db.students.find().pretty()
 
 => {
        "_id" : ObjectId("5c3641a706dfefd94bf2eda7"),
        "name" : "Devan",
        "gradeAverage" : "B",
        "projects" : [
                {
                        "_id" : ObjectId("5c3641a706dfefd94bf2eda5"),
                        "title" : "Project3",
                        "unit" : "Mongoose"
                },
```
<br />

## Your turn!


 Follow this [link]https://mongoosejs.com/docs/queries.html to access more Mongoose queries. Work with a partner and use at least 3 different queries in your database.
 
 <br />
 
 ## CRUD with Mongoose
 
 The Express generator already a ` routes` folder for us so let's go inside and create a file to hold our routes for Students & Projects. 
 
 We will set up our routes now and test them in Postman. If you don't have Postman yet, follow this [link]https://www.getpostman.com/ to download. 
  
  
 Next, we need to let Express know where to grab our routes. Inside of your `app.js` please add
 ```
 var projectsRouter = require('./routes/projects')
 var studentsRouter = require('./routes/students')
 ```
 
 this tells express where to grab your routes from.
 
 Within your routes/students.js file add these routes
 
 Students Get Route
 
 ```
router.get('/', (request, response) => {
  StudentModel.find({})
    .then((students) => {
      response.send({ 
        students: students
      })
    })
    .catch((error) => {
      console.log(error)
    })
})

module.exports = router
```
show route

```
router.get('/:id', (request, response) => {
  const id = request.params.id

  StudentModel.findById(id)
    .then((student) => {
      response.send({
        student: student
      })
    })
})
```

update route

```
router.put('/:studentId', (request, response) => {
  const studentId = request.params.id
  const updatedStudent = request.body

  StudentModel.findOneAndUpdate(studentId, updatedStudent, {new: true})
    .then((student) => {
      console.log(`${student.name} updated!`)
    })
    .catch((error) => {
      console.log(error)
    })
})
```

delete route
```
router.delete('/:studentId', (request, response) => {
  const studentId = request.params.studentId

  StudentModel.findOneAndDelete(studentId)
    .then(() => {
      console.log('student deleted')
    })
    .catch((error) => {
      console.log(error)
    })
})
```
Now let's move on to our embedded route Projects. First we need to add the following:
```
var express = require('express');
const router = express.Router({ mergeParams: true })
const { StudentModel } = require('../db/schema')
const { ProjectModel } = require('../db/schema.js')
```
Since this is an embedded route we need to require the Student student and allow access to the params from the route(i.e. the studentId)

get route
```
//get route
router.get('/:projectId', function (req, res, next) {
    projectId = req.params.projectId
    StudentModel.findById(req.params.studentId).then((student) => {
            res.send(student.projects.id(projectId))
    })
    
});
```
