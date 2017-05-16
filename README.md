<!--
Creator: Cory Fauver
Last Edited By: Brianna Veenstra (WDI34)
Market: SF
-->

![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png)

# Mongoose

### Warmup

Fill in the express project file structure organizer (printed sheet).  For each file or directory on the sheet, list:
- a short summary of the purpose of the file or directory
- whether the file or directory is most related to the client, the server, or the database

You won't know what some of the database files do yet, but that's okay. Make notes about them as you see them come up today.

[Here's a link to a filled-in sheet where you can add comments if you'd like.](https://docs.google.com/document/d/1HQwKMzNMf6Rn80n-32cTEtDi4wgkxPQV5WdpDw36PhI/edit?usp=sharing)

### Why is this important?
<!-- framing the "why" in big-picture/real world examples -->
*This workshop is important because:*

We want user data that lasts (or "persists") even when we make changes to the code on our servers! Databases will give us more power to store persistent data even when servers restart.

There are *many* kinds of databases that are optimized for different things.  We'll start with MongoDB. MongoDB is a widely-used noSQL database, and using it will help you understand how many databases work.

We'll also take advantage of Mongoose, a library that makes it easier to use MongoDB with express projects.


### What are the objectives?
<!-- specific/measurable goal for students to achieve -->
*After this workshop, developers will be able to:*

- Describe the relationship between schemas and models in Mongoose.
- Create Mongoose model instances.
- Using Mongoose, integrate a MongoDB database with an Express project.


### Where should we be now?
<!-- call out the skills that are prerequisites -->
*Before this workshop, developers should already be able to:*

- Use Express to configure a server's responses to various HTTP verbs on various routes.
- access data that comes in on a request from the client-side (`req.body` or `req.params`).

## MongoDB versus Mongoose

![tumblr_nbhme6bafu1s02vreo1_500](https://cloud.githubusercontent.com/assets/4304660/16811532/b08865a8-48dd-11e6-9474-c114b2e8a00d.gif)

`MongoDB` is a no-SQL database. It is responsible for storing data in containers and making sure that the data is safe and organized. MongoDB stores data in BSON, "a binary JSON format", and it has a JavaScript API. We'll see SQL databases later that store data in a format more like an Excel spreadsheet.

`Mongoose` is a library or "wrapper" that gives us a bunch of convenience methods for working with MongoDB records (kind of like jQuery's convenience methods for manipulating the DOM). Generally we will not be interacting _directly_ with MongoDB, instead we'll be working through `mongoose`.


## Schemas and Models

Mongoose presents us with two key concepts for how we create and store data in our MongoDB database.

**[Schema](http://mongoosejs.com/docs/guide.html)**: A Schema is a diagram or blueprint for what every object in the noSQL database will contain. It does not include any methods, just placeholders for what data you will eventually store. Here's an example of a simple Address Book mongoose schema:

```js
var ContactSchema = new Schema({
    firstName: String,
    lastName: String,
    address: String,
    phoneNumber: Number,
    email: String,
    professionalContact: Boolean
});
```

With the above Schema, we can expect that all of our Address Book entries would have a first name, last name, address, and email address in the form of Strings. We can count on the phoneNumber to always be accepted, stored, and returned as a number. Lastly, the boolean value of Professional Contact will always be a true or false. A Schema has no functionality. It simply defines the shape of the data that we will expect when we work with contacts.

**[Model](http://mongoosejs.com/docs/models.html)**: A mongoose model is compiled from a Schema. It takes in the structure and shape of a Schema and adds the capacity to perform actions such as reading, saving, updating, etc. The Schema is just an inert mould to make sure that the models will hold the data consistently. A model is actually capable of creating new entries in a database and retrieving data from the database. Here's how you'd make a Contact model out of our Contact Schema:

```js
var Contact = mongoose.model('Contact', ContactSchema);
```

![image](https://i.chzbgr.com/full/7986468352/hE55E1B66/)

Factory metaphor: Imagine a factory that has a mold for making rubber ducks. The mold would be the Schema. The machine that is capable of putting the different colored plastic into the mold, pressing it, and delivering a new toy to the world would be the model. The toy itself would be the data that would now be stored in your database.

![image](https://cloud.githubusercontent.com/assets/6520345/18133637/3e2d48e0-6f50-11e6-80c7-0336334d8c91.png)

## MongoDB & Mongoose setup

Let's do a quick activity and get Mongoose and MongoDB running.

1. Assuming you already have MongoDB installed (you did this at installfest), to get started using mongoose in a project, we have to install it to add it to a project's `package.json`:

  ```bash
    npm install --save mongoose
  ```

2. Next we need to `require` Mongoose in our project and `connect` to the MongoDB service (it could be local or hosted). We can do this in `server.js`, or separate the code a little more by using a `models` directory and connecting everything in `models/index.js`.

  ```js
    var mongoose = require('mongoose');
    mongoose.connect('mongodb://localhost/todo-app');
  ```

<details>
<summary>What's a connection string?</summary>
The <code>'mongodb://localhost/todo-app-demo'</code> is a database URL.  Here, it's a local database named <code>todo-app-demo</code>. You can name the system whatever you like and it will be created as soon as you save some data to it.
</details>
<br>

3. Finally, we need to run the MongodDB service. Generally you will want it open in a separate tab, running in the background.

  ```bash
    mongod
  ```

> Note: If you already have an instance of MongoDB running, you'll get an error at this step. If that's the case, you can move on to the next step, since MongoDB is already running!

Running your MongoDB service is a lot like running your Express Server!


## Todo App Integration
Once you've finished the above steps, here's how you would set up an Express application with a "Todo" model (so we can start CRUDing todos!). Look in the directory `starter-code` for a starting point.

1. We'll need a `Todo` model, which we will set up in a file called `todo.js`....

2. In your model file (e.g. `todo.js`), create the model **schema**, use it to make a **model**, and export the model so that you can require it in other parts of your app.

<details><summary>click for code</summary>

```
var mongoose = require('mongoose'),
    Schema = mongoose.Schema;

var TodoSchema = new Schema({
    task: String,
    description: String
});

var Todo = mongoose.model('Todo', TodoSchema);

module.exports = Todo;
```

</details>
<br>


3. In `models/index.js`, require your model.

<details>
<summary>click for code</summary>

```
// models/index.js
// require runs the code from the given file and returns its exports
var Todo = require('./todo');
```

</details>
<br>


4. Next in `models/index.js`, export the new model.

<details><summary>click for code</summary>

```
// models/index.js
// require runs the code from the given file and returns its exports
var Todo = require('./todo');
// NEW LINE:
exports.Todo = Todo;

```

This will allow us to use the `Todo` model in `server.js`.
</details>
<br>



#### Database IDs and data types

Most databases also require that we specify the data type for each attribute.  In mongoose we can use data types from JavaScript, such as String, Number, and Array. Here's a list of all the [available data-types](http://mongoosejs.com/docs/schematypes.html) in mongoose.

Let's look at this example from the starter-code for these notes.

```js
// models/person.js
var mongoose = require('mongoose'),
  Schema = mongoose.Schema;

var personSchema = new Schema({
    firstName: String,
    lastName: String,
    height: Number,
    superPower: String,
    weakness: String,
    isExcited: Boolean
});

var Person = mongoose.model('Person', personSchema);

module.exports = Person;
```

> In the above, note how we've assigned **String**, **Number** and even a **Boolean** as the data types for this Schema.

Once the model object is exported from its individual file, you'll want to `require` it to use it in another file.

Here's an example of how we could require the model directly into a server file:

```js
  // server.js
  var Person = require('./models/person');
  // now Person stores the Person model from the other file!
```

Here's a slightly more complex example using the structure with a `models/index.js` file to group together all the models:  

```js
  // models/index.js
  var PersonModel = require('./models/person');
  module.exports = {
    Person: PersonModel
  }
  // or
  // module.exports.Person = PersonModel;
```

... and in `server.js`:  

```js
  // server.js
  var db = require(`./models`); // grab the export object from models/index.js
  // now db.Person stores the Person model from the models/person.js file
```



## CRUD Operations with Mongoose

Until now, when we wanted to access or manipulate stored data on our server, we worked with an array. We were sending around the whole array, finding single objects in an array, adding objects to an array, and deleting elements from an array.

<details>
  <summary>At what API route did we complete each of the above?</summary>
  <ul>
    <li>sending along the whole array: GET /todos</li>
    <li>finding single objects in an array: GET /todos/:id</li>
    <li>adding objects to an array: POST /todos</li>
    <li>deleting elements from an array: DELETE /todos/:id</li>
  </ul>
</details>
<br>



Luckily, Mongoose provides methods to access the database data which will help us accomplish these tasks.

#### Get all todos: `.find()`

We can use <a href="http://mongoosejs.com/docs/api.html#model_Model.find">.find()</a> to get all documents in the collection.

<details><summary>click for code</summary>

```
// get all todos
app.get('/api/todos', function(req, res) {
// find all todos in db
    db.Todo.find({}, function(err, allTodos) {
        res.json({ todos: allTodos });
    });
});
```

> Note: We can also use `.find()` to get a specific set of documents in the collection (rather than ALL documents) by setting conditions. Read more <a href="http://mongoosejs.com/docs/api.html#model_Model.find"  target="_blank">in the docs</a>.

</details>
<br>


#### Create new todo: `new` and `.save()`

We've seen the `new` keyword before! It creates new instances of an object. We use it here to create new instances of our `Todo` model. We then call `.save()` to store the new todo in our database.

<details><summary>click for code</summary>


```javascript
// create new todo
app.post('/api/todos', function(req, res) {
    // create new todo with form data (`req.body`)
    var newTodo = req.body;
    db.Todo.create(newTodo, function(err, createdTodo) {
    	if(err){return console.log(err)}
    	res.json(createdTodo);
    });
});
```

</details>
<br>


#### Get one todo: `.findOne()`

We can use <a href="http://mongoosejs.com/docs/api.html#query_Query-findOne">.findOne()</a> to return the first document in the collection that matches certain criteria. In this case, we're looking for a todo that has a certain `_id`.

<details><summary>click for code</summary>  


``` javascript
// get one todo
app.get('/api/todos/:id', function(req, res) {
    // get todo id from url params (`req.params`)
    var todoId = req.params.id;

    // find todo in db by id
    db.Todo.findOne({ _id: todoId }, function(err, foundTodo) {
       	if(err){return console.log(err)}
        res.json(foundTodo);
    });
});
```

> Note: The <a href="http://mongoosejs.com/docs/api.html#model_Model.findById" target="_blank">.findById()</a> method will also return a single document matching a specified id field.

</details>
<br>


#### Update todo: `.findOneAndUpdate()`

Similar to the last example, we can use `.findOneAndUpdate()` to find the document with a certain `_id` and  then update the document in one method.

<details><summary>click for code</summary>

```
// update todo
app.put('/api/todos/:id', function(req, res) {
    // get todo id from url params (`req.params`)
    var todoId = req.params.id;

    // create an updateTodo object from req.body
	var updateTodo = {
	  task: req.body.task,
      description: req.body.description
    }

    db.Todo.findOneAndUpdate({ _id: todoId }, updateTodo, { new: true}, function(err, updatedTodo){
      if(err){return console.log(err)}
	  res.json(updatedTodo);
    });
});
```

</details>
<br>


#### Delete todo: `.findOneAndRemove()`

The <a href="http://mongoosejs.com/docs/api.html#model_Model.findOneAndRemove" target="_blank">.findOneAndRemove()</a> method takes care of finding the document with a certain `_id` and removing it from the database.

<details><summary>click for code</summary>

```
// delete todo
app.delete('/api/todos/:id', function(req, res) {
    // get todo id from url params (`req.params`)
    var todoId = req.params.id;

    // find todo in db by id and remove
    db.Todo.findOneAndRemove({ _id: todoId }, function(err, deletedTodo) {
        res.json(deletedTodo);
    });
});
```

> Note: Another way to remove the document is by finding the document first (using `.findOne()` or  `.findById()`) and calling <a href="http://mongoosejs.com/docs/api.html#model_Model.remove" target="_blank">`.remove()`</a>.

</details>
<br>


## Independent Practice
Practice the skills covered in this workshop with the [Mongoose books training](https://github.com/generalassembly-atx/mongoose-books-app). We'll be doing **Sprint 1** only today.

## Closing Thoughts
- Why is Mongoose useful?
- Compare and contrast a schema with a model.

## Additional Resources
* [MongooseJS](http://mongoosejs.com/)
* [Mongoose Getting Started](http://mongoosejs.com/docs/)
