Connection to MongoDB
=====================

Connect to different MongoDB database depending on enviornment

```coffee
# server.coffee
mongoose = require "mongoose"
app.configure "development", ->
	mongoose.connect "localhost", "app-dev"
```

Models
======

```coffee
# Todo.coffee
mongoose = require "mongoose"
Schema = mongoose.Schema

todoSchema = mongoose.Schema
	name: String
	...

module.exports = mongoose.model "Todo", todoSchema
```

Don't need to put in `connection.on "open", ->` as [Mongoose will buffer queries until connection is made](http://stackoverflow.com/a/11910932/292291)

Relationships
-------------

`TodoList`s has many `Todo`s: 

```coffee
# Todo.coffee
todoSchema = mongoose.Schema
	...
	list: { type: Schema.Types.ObjectId, ref: "TodoList" }
```

```coffee
# TodoList.coffee
listSchema = mongoose.Schema
	name: String
	todos: [{ type: Schema.Types.ObjectId, ref: "Todo" }]
```

**Usage**: 

```coffee
# define a list
l1 = new TodoList
	name: "List 1"
# define child todos
t1 = new Todo 
	...
	list: l1._id # link to parent
t2 = new Todo 
	...
	list: l1._id
t3 = new Todo 
	...
	list: l1._id
# link from parent to children
l1.todos.push t1._id, t2._id, t3._id
```

**Tip: save in parallel**

```coffee
# using async.js
async.parallel [
	(done) -> l1.save(done)
	(done) -> t1.save(done)
	(done) -> t2.save(done) 
	(done) -> t3.save(done)
	(done) -> t4.save(done)
], (err) -> ...
```