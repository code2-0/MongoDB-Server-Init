# MongoDB-Server-Init
 Documentation for mongoDB server initialization

## **Necessary packages:**
 To create MongoDB server APIs there need to `install` some of *node packages*. 
 * **Express**: Basically APIs are written by using `express`. Express make it easy to write APIs than *vanilla JavaScript*. So, First of all I need to Install *express*.
 * **MongoDB**: If I want to use mongodb as database manament system, I have to install `mongoDB`.
 * **Cors**: `Cros` is an important package for loading data by fetching APIs. Usually, I host the server and frontend project in different platform. As a result, when I will load data by using APIs from different platform there is creared some barriers to load data from other platform, this is called `Cros`. To solve fetching problem from cros platform/ different platfom I have to use `Cors` package.
 * **DotEnv**: `DotEnv` is an essential package for realtime server application. This page help to emplement `environment variable` in the application.
 * **Nodemon**: `Nodemon` works only locally on the computer to run the application automatically after change in any file. `node fileName.js` command run the file once. But, when I work on a large application, there need to live preview after every change. As solution, `nodemon` package is used.
 * **JWT**: `JWT` package is used realtime application to ensure security of data.
 ### **Installation Commands:**
 ```
 npm i express
 ``` 
 ```
 npm i mongodb
 ``` 
 ```
 npm i cors
 ``` 
 ```
 npm i dotenv
 ``` 
 ```
 npm i nodemon
 ``` 
 ```
 npm i jwt
 ``` 
 I can Install a number of packages at a time. Such as:
 ```
 npm i express mongodb cors dotenv nodemon
 ```

## **Initial SetUp:**
After installation of all necessary packages I need to create some files. *`.env`* (for environment variables), *`.gitignore`* (for ignoring unnecessary files pushing on the github) and *`index.js`* (where I will write my APIs).

### ***.ENV:***
`.env` file contains the secret data, which should not share with others. It includes database usename password which is secret. Structure of `.env` file:
```
KEY=VALUE
KEY=VALUE
```
#### **Use of ENV:**
I store all secret things into the `.env` file. when I will use these, use as:
```js
process.env.KEY
```

### ***.GITIGNORE:***
`.gitignore` file includes those file or folder/directory names which I don't want to push on github repository. It should have include `.env` file. Such as :
```js
.env // a file
node_modules //a folder/directory
```

### ***INDEX.JS:***
This is the actual file where I will write all codes and APIs. Here need to import/require installed packages and need some initial setup. Such as:
```js
const express = require("express");
const cors = require("cors");
const dotenv = require("dotenv");
const { MongoClient, ServerApiVersion } = require('mongodb');
const objectId = require("mongodb").ObjectId;

const app = express();
dotenv.config()
app.use(cors());
app.use(express.json());
const port = process.env.PORT || 5000;

const uri = `mongodb+srv://${process.env.DB_USER}:${process.env.DB_PASS}----------------------------------------------------------`;
const client = new MongoClient(uri, { useNewUrlParser: true, useUnifiedTopology: true, serverApi: ServerApiVersion.v1 });

const runAPIs = async() => {
  try{
    await client.connect();
    const database = client.db('redux-emajon');
    const productsCollection = database.collection("products");

  }finally{

  }
}
runAPIs().catch(console.dir);

app.get('/', (req, res) => {
  res.send("This server is running!")
});

app.listen(port, ()=>{
  console.log('Server is Running!!');
})
```

### ***`GET REQUEST API:`***
`GET` is *API* request method. To fetch or load data from server we have to have a `get` method. We can load all data of a database collection, limited number of data or a specific data by `get` method. If we want to load just one data we have to use `findOne()` method, or, if we want to load multiple data than use `find()` method. Code:
```js
  app.get('/path_name/:dynamic_route', async(req, res) => {
    const result = await collectionName.find({}).toArray();
    // empty object ({}) is use inside find() to get all data of this collection
    //.toArray() is called to put all data in an array.
    res.json(result);
  })
```
- #### **PARAMS:**
  Dynamic pathName. Sometimes we need to show data at the dynamic routes. So, we need to fetch the data dynamically. `params` returns the dynamic pathname which is dynamically called from the frontend. To get the pathname we have to write: 
  ```js
    // blow /:id or /:email reffers dynamic pathname.
  // if I have mongoDB ObjectId/ _id for finding:
  // here _id is a unique for every data so it must get single one, so use findOne();
  app.get('/blog/:id', async(req, res)=>{
  const id = req.params.id;
  const query = {_id : ObjectId(id)};
  const result = await postCollection.findOne(query);
  
  });

  // If I have except _id any other property:
  app.get('/blog/:email', async(req, res)=>{
    // find by email(any others finding like this except _id);
  const email = req.params.email;
  const query = {email : email};
  const result = await postCollection.find(query);
  });
  ```
- #### **QUERY:**
  If we need to find data with single or multiple parameter we can use `query`. If we pass `query` from frontend fetch we will find it in express. `req.query` returns an object, which have those propertis and values passed from frontend `get` request.
  ```js
   app.get('/posts', async (req, res) => {
    const options = req.query;
    const result = await postCollection.find(options).toArray();
   })
   ```
- #### **SORT():**
  If we need to sort data than:
  ```js
    app.get('/path', async (req, res) => {
      const result = await collectionName.find({}).sort(_id:-1).toArray();
      //sort new data show first
      // by default old data show first
    });
  ```
- #### **LIMIT():**




### ***`POST REQUEST API:`***
`POST` method is used to post or insert data to database. Code like:

`mongoDB` deals with **`object`**. So our requested data should be an `object`. If we have a single object to insert than use `insetOne()` method. Like:
```js
  app.post('/blog', async (req, res) => {
    const data = req.body;
    const result = await collectionName.insertOne()
  });
```
If we have multiple objects to insert at time than use `insertMany()` method and the objects should be into an array. Like:
```js
  app.post('/blogs', async (req, res) => {
    const data = [
      {name:"Md Ashik Ali", media:'github'},
      {name:"Md Sagor Khan", media:'email'},
      {name:"Abdur Rahman", media:'facebook'},
    ];
    const result = await blogsCollection.insertMany(data);
  });
```
- #### **BODY:**
  When we send a `post` request from frontend, we have to send necessary data through `body` property. And those data we get in `req` parameter. 
  ```js
  app.post('/some', async (req, res) => {
    const data = req.body;
  })
  ```

### ***`PUT REQUEST API:`***
`put` method is used to ***update*** or, ***modify*** data. To modify single item use `updateOne(finding-option, update-option)`. Code like:
```js
  app.put('/some/:id', async (req, res) => {
    const query = {_id : ObjectId(req.params.id)};
    const update = {$set : req.body};
    const result = await collectionName.updateOne(query, update);
  })
```

### ***`DELETE REQUEST API:`***
`delete` method is used to delete data from database. If we want to delete single data than use `deleteOne()` method. Code:
```js
  app.delete('/some/:id', async (req, res)=>{
      const query = {_id : ObjectId(req.params.id)};
      const result = await collectionName.deleteOne(query);
      res.json(result)
    });
```
If we want to delete multiple data at a time than use `deleteMany()` method. Code:
```js
  app.delete('/some', async (req, res)=> {
      const {platform} = req.query;
      const result = await collectionName.deleteMany({platform: platform});;
      res.json(result)
    });
    // using _id
    app.delete('/some', async(req, res)=>{
      const ids = req.query.ids.split(',');
      const query = ids.map(id => ObjectId(id));
      const result = await collectionName.deleteMany({_id : {$in : query}});
      res.json(result);
    });
```

### ***`UPSERT REQUEST API:`***
`upsert` method nothing but a `post` method. When we don't know this type of data is in our database collection or not, if data does not exist, than we need to insert, if we already have than no need to insert, because it already exist. And than we should use `upsert` option inside `post` method. here request method will `post` and insert method will be `update` these two are combindly called as **`upsert`**. Code:
```js
app.post('/member', async(req, res) => {
      const query = {email : req.body.email};
      const update = {$set : req.body};
      const options = { upsert: true };
      const result = await memberCollections.updateOne(query, update, options);
      res.json(result);
    });
```
This method, if find data than update that and if do not find than insert data.
