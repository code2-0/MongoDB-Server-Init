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

const uri = `mongodb+srv://${process.env.DB_USER}:${process.env.DB_PASS}@cluster0.muk27.mongodb.net/?retryWrites=true&w=majority`;
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
`GET` is *API* request method. To fetch or load data from server we have to have a `get` method. We can load all data of a database collection, limited number of data or a specific data by `get` method. Code:
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
  app.get('/blog/:id', async(req, res)=>{
    // above /:id reffers dynamic pathname.
  const id = req.params
  })
  ```
- #### **QUERY:**
  If we need to find data with single or multiple parameter we can use `query`. If we pass `query` from frontend fetch we will find it in express. `req.query` returns an object, which have those propertis and values passed from frontend `get` request.
  ```js
   req.query
   ```
- #### **SORT():**
- #### **LIMIT():**
- #### **TOARRAY():**



### ***`POST REQUEST API:`***
- #### **BODY:**

### ***`PUT REQUEST API:`***

### ***`DELETE REQUEST API:`***

### ***`UPSERT REQUEST API:`***