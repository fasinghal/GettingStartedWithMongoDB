# Getting Started With MongoDB
This repository contains notes related to MongoDB.
<hr>

### Installation Steps in Windows

1. Go to https://www.mongodb.com
2. Go to downloads
3. Select Community server OR navigate to https://www.mongodb.com/download-center?jmp=nav#community
4. Select the first version appears on the dropdown menu i.e. **Windows server 2008 R2 64-bit and later, with SSL support x64**
5. Once you have downloaded the installer, open it and click next, select **complete setup** option to install.

<br>

### Starting the MongoDB Server

- After the software is installed, navigate into command prompt and boot up the server. 
  - Navigate into program files -> MongoDB -> server -> 3.6 -> bin
    This folder contains all executables to start up the server and connect to it
    Example-> mongod.exe => This starts local MongoDB database
    
<img src = "https://github.com/patilankita79/GettingStartedWithMongoDB/blob/master/Screenshots/Exploring%20bin%20folder.png">

- Before we execute any executable, we need to create a directory where all of our data can get stored. For this, I have created a folder  named **mongo-data** in my user directory where the data is going to get stored. 
  - This is the path we need to specify when we execute mongod.exe (We need to tell the mongo where to store the data)

<img src = "https://github.com/patilankita79/GettingStartedWithMongoDB/blob/master/Screenshots/Command%20to%20start%20%20server.png" >

- By executing this command, you start up the server
<br>
<img src ="https://github.com/patilankita79/GettingStartedWithMongoDB/blob/master/Screenshots/Server%20has%20started.png">

- Now, the server has started on port - 27017
- Let's create and read data. Open another command prompt and go to bin folder insider MongoDB folder. Run **mongo.exe** This connects to local MongoDB database
- You can try inserting data to database and try fetching inserted data for testing purpose.

<img src = "https://github.com/patilankita79/GettingStartedWithMongoDB/blob/master/Screenshots/mongo.exe.png">

<hr>

## NoSQL Database

- In relational databases, we have relation/table whereas in NoSQL databases we have collection
- In relational databases, we have rows/records whereas in NoSQL databases we have document
- In relational databases, individual properties are called columns. It is schema based. So whatever you specify in the schema has to be present for all the records. In case of NoSQL databases, the documents in the collection don't need to have the same properties.The properties are called fields


### Connecting to MongoDB database from inside of NodeJS application

- Note: Make sure to start your mongodb server (Refer <a href = "">this</a>)

- NPM module used -> node mongodb native (https://github.com/mongodb/node-mongodb-native)

```
npm install mongodb@2.2.5 --save
```
- MongoClient from **mongodb library** lets you connect to the mongo server and issue command to manipulate the database
<br>

**Code Snippet => Simply connecting to MongoDB database from inside of NodeJS aplication**

```
// Loading the library
const MongoClient = require('mongodb').MongoClient;

// Connection URL (to connect to database/ URL of the location where database lives)
const url = 'mongodb://localhost:27017/TodoApp';

// Connecting to the database using MongoClient
MongoClient.connect(url, (err, db) => {
  if(err) {
    return console.log('Unable to connect to MongoDB server');
  }

  console.log('Connected to MongoDB server.');

  // Close the connection with MongoDB Server
  db.close();
});

```

**You can give URL of the database that doesn't exist. MongoDB will form the connection but will not create a database, until we add data to it.**

<br>
## Insert Data into database

**Code Snippet => Inserting a document into MongoDB database using NodeJs**
<br>
```
// loading the library
const MongoClient = require('mongodb').MongoClient;

// Connection URL (to connect to database/ URL of the location where database lives)
const url = 'mongodb://localhost:27017/TodoApp';

// Connecting to the database using MongoClient
MongoClient.connect(url, (err, db) => {
  if(err) {
    return console.log('Unable to connect to MongoDB server');
  }
  console.log('Connected to MongoDB server.');

  // Inserting new record to collection
  db.collection('Todos').insertOne({
    text: 'My first to-do',
    completed: false
  }, (err, result) => {
    if(err) {
      return console.log('Unable to insert todo', err);
    }

    // result Contains the result document from MongoDB
    // ops Contains the documents inserted with added _id fields
    console.log(JSON.stringify(result.ops, undefined, 2));
  });


  // Close the connection with MongoDB Server
  db.close();
});

```

**Output of the above code snippet in command prompt**
<br>
<img src = "https://github.com/patilankita79/GettingStartedWithMongoDB/blob/master/Screenshots/Inserting%20a%20document%20in%20MongoDB%20database.png">
<br>

**Now, checking the generation of collection and document in MongoDB Compass**. <br>
In the following image, 
- Database Name is TodoApp
- Collection is Todos
- Document is the data record which is inserted.
<br>
<img src = "https://github.com/patilankita79/GettingStartedWithMongoDB/blob/master/Screenshots/Document%20entry%20in%20the%20database.png">

<br>

Now, let's try adding one more collection - Users to TodoApp database

```
  // Inserting new record to collection - Users
  db.collection('Users').insertOne({
    name: 'Ankita',
    age: 23,
    location: 'TX'
  }, (err, result) => {
    if(err) {
      return console.log('Unable to insert user', err);
    }
    console.log(JSON.stringify(result.ops, undefined, 2));
  });
```

<img src = "https://github.com/patilankita79/GettingStartedWithMongoDB/blob/master/Screenshots/Inserting%20Users%20collection.png">
<img src = "https://github.com/patilankita79/GettingStartedWithMongoDB/blob/master/Screenshots/UserCollection.jpg">
<br>
<hr>

## Exploring ObjectId

- ObjectId is randomly generated id used to uniquely identify every document in MongoDB databases
- MongoDB databases can be scaled out. Scaling out means you add more database servers to handle the extra load.
- When we use randomly generated id we don't need to constantly communicate with other database servers to check what the highest incrementing value is.
- ObjectId is a 12 byte value where the first 4 bytes are a timestamp, next 3 bytes are machine identifiers (If 2 computers generate ObjectIds, then they will have machine identifier to ensure that id generated is unique), next 2 bytes are process id and lastly we have 3 bytes counter.

<hr>

## Fetch data from database/ Querying data from NodeJS

- Collection is accessed using find(). find() returns mongodb cursor (Pointer to documents)

```
// loading the library
const MongoClient = require('mongodb').MongoClient;

// Connection URL (to connect to database/ URL of the location where database lives)
const url = 'mongodb://localhost:27017/TodoApp';

// Connecting to the database using MongoClient
MongoClient.connect(url, (err, db) => {
  if(err) {
    return console.log('Unable to connect to MongoDB server');
  }
  console.log('Connected to MongoDB server.');

  // Access the collection
  db.collection('Todos').find().toArray().then((docs) => {
    console.log('Todos');
    console.log(JSON.stringify(docs, undefined, 2));
  }, (err) => {
    console.log('Unable to fetch todos', err);
  });

});

```

<img src = "https://github.com/patilankita79/GettingStartedWithMongoDB/blob/master/Screenshots/Querying%20the%20data%20from%20NodeJS.png">

<hr>

## Deleting documents

**Using deleteMany()** -> targets many documents and remove them
<br>
For this scenario, I have cloned one document entry thrice in the database.
<img src = "https://github.com/patilankita79/GettingStartedWithMongoDB/blob/master/Screenshots/InkeddeleteMany_1_LI.jpg">

```
// loading the library
// const MongoClient = require('mongodb').MongoClient;
const {MongoClient, ObjectID} = require('mongodb');

// Connection URL (to connect to database/ URL of the location where database lives)
const url = 'mongodb://localhost:27017/TodoApp';

// Connecting to the database using MongoClient
MongoClient.connect(url, (err, db) => {
  if(err) {
    return console.log('Unable to connect to MongoDB server');
  }
  console.log('Connected to MongoDB server.');

  // deleteMany() -> targets many documents and remove them
   db.collection('Todos').deleteMany({text:'Go to Walmart'}).then((result) => {
     console.log(result);
   });

  // db.close();
});

```

Output,
<img src = "https://github.com/patilankita79/GettingStartedWithMongoDB/blob/master/Screenshots/InkeddeleteMany_2_LI.jpg">
<br>

**Using deleteOne()** -> targets one documents and removes it. <br>
For this scenario, I have cloned a document entry twice in the database<br>
<img src = "https://github.com/patilankita79/GettingStartedWithMongoDB/blob/master/Screenshots/InkeddeleteOne_1_LI.jpg">

```
// loading the library
// const MongoClient = require('mongodb').MongoClient;
const {MongoClient, ObjectID} = require('mongodb');

// Connection URL (to connect to database/ URL of the location where database lives)
const url = 'mongodb://localhost:27017/TodoApp';

// Connecting to the database using MongoClient
MongoClient.connect(url, (err, db) => {
  if(err) {
    return console.log('Unable to connect to MongoDB server');
  }
  console.log('Connected to MongoDB server.');

   // deleteOne() -> targets one documents and removes it
   db.collection('Todos').deleteOne({text:'Submit job applications'}).then((result) => {
     console.log(result);
   });

  // db.close();
```

Output,
<img src = "https://github.com/patilankita79/GettingStartedWithMongoDB/blob/master/Screenshots/InkeddeleteOne_2_LI.jpg">
<br>
**Using findOneAndDelete()** -> removes individual item and also returns those values. <br>

<img src = "https://github.com/patilankita79/GettingStartedWithMongoDB/blob/master/Screenshots/findOneAndDelete_1.png">

```
// loading the library
// const MongoClient = require('mongodb').MongoClient;
const {MongoClient, ObjectID} = require('mongodb');

// Connection URL (to connect to database/ URL of the location where database lives)
const url = 'mongodb://localhost:27017/TodoApp';

// Connecting to the database using MongoClient
MongoClient.connect(url, (err, db) => {
  if(err) {
    return console.log('Unable to connect to MongoDB server');
  }
  console.log('Connected to MongoDB server.');

   // findOneAndDelete() -> removes individual item and also returns those values
  // targetting todos with completed: false
  db.collection('Todos').findOneAndDelete({completed:false}).then((result) => {
    console.log(result);
  });

  // db.close();
```

Output,
<img src = "https://github.com/patilankita79/GettingStartedWithMongoDB/blob/master/Screenshots/InkedfindOneAndDelete_2_LI.jpg">
<img src = "https://github.com/patilankita79/GettingStartedWithMongoDB/blob/master/Screenshots/findOneAndDelete_3.png">
