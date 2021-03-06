# Mongoose

A mongoose is a honey badger-esque rodent that fights snakes, like a squrrel that fights cobras. Only slightly less cool, is [mongoosejs][mongoosejs]. Mongoosejs is your handy dandy ORM (object relational mapping) resource for mongo.

Like Knex and Bookshelf, Mongoose will give you the power to...
  - intereact with your database from your node code
  - create models for your data
  - query your database using your models and javascript

Unlike Knex and Bookshelf...
  - Mongoose is complete by itself and doesn't require multiple installs to get the job done

### Installing and using mongoose

Moongoose is pretty intuitive, if you guessed `npm install mongoose` and then `const mongoose = require('mongoose')`, you'd be right. The only slightly tricky thing about using mongoose is that its promise library is depricated, so you should probably add something like `mongoose.Promise = Promise`, so that  you can use native es6 javascript promises.

### Creating models

Models are even more important in mongo than they are in SQL. Since document-based databases don't enforce any particular schema, your model will be the primary method for defining and validating the structure of your data.

The [docs][docs] can be pretty helpful when creating models. Mongoose models accept two arguments, a string, which will be the name of our model, and a schema, which is an object the defines the keys of our document and their types. For example, an extremely simple model might look like so:
```
const Student = mongoose.model('Student', {
  name: String,
  age: Number,
  skills: [String] //<--- an array of strings
})
```

Your schema can also inclue a number of different options, including settting values to  lowercase, setting values as required, etc. See more in the [docs](http://mongoosejs.com/docs/schematypes.html).

```
const Student = mongoose.model('Student', {
  name: { type: String,
          required: true,
          match: /^[a-zA-Z]+$/, 'your name may only contain letters'
  },
  age: Number,
  skills: [String] //<--- still an array of strings
})
```
### Connecting your mongo database to your app

In order to connect your database to your app, you will need to connect to your databse first, and then put your express app.listen logic inside the "then" stament, like so:
```
w/mongoose:
const mongoose = require('mongoose');
const PORT = process.env.PORT || 3000 

const MONGODB_URL = 'mongodb://localhost:27017/sockDB' //<-- database about socks

//...
mongoose.connect(MONGODB_URL)
  .then(() => {
    app.listen(PORT, () => {
    console.log(`Hey, I'm listening on port ${PORT}`);
    })
  })
  .catch(console.error)
```

### Using your mongoose models to write queries

Assuming you have a model for 'Student', you can write a query to find all students by writing:
```
  Student
  .find()
  .then(students => console.log(students))
  .catch(err)
```

You could also create a new student:
```
const studentInfo = {//info goes here}

  Student
    .create(studentInfo)
    .then((student) => res.send("done"))
    .catch(err)

```

Checkout the other helpful mongoose query methods in the [docs](http://mongoosejs.com/docs/queries.html)



## Exercise

Go big or go home. Your exercise is to clone the repo at https://github.com/C-Stein/mean-mongoose-zoo and write a server for it using Mongoose. You will need to create

- an express server connected to a mongo database
- an Animal model created with mongoose
- get, post, patch, and delete routes that connect to the angular front end provided and interact with your mongo database
- don't be afraid to check out the mongoose docs and find the best possible query methods for your app

helpful hint: include the following middleware if you run into issues with CORS headers
```
app.use(function(req, res, next) {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  res.header("Access-Control-Allow-Methods", "GET, POST,HEAD, OPTIONS,PUT, DELETE, PATCH");
  next();
});
```
[mongoosejs]: http://mongoosejs.com/
[docs]: http://mongoosejs.com/docs/guide.html
