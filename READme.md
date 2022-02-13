

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [Backend](#backend)
  - [Creating files, writing boiler plate, and establishing connection to database](#creating-files-writing-boiler-plate-and-establishing-connection-to-database)
  - [Writing Schemas - models folder](#writing-schemas-models-folder)
  - [Add API endpoint routes so the server can be used to perform the CRUD Operations - routes folder](#add-api-endpoint-routes-so-the-server-can-be-used-to-perform-the-crud-operations-routes-folder)

<!-- /code_chunk_output -->


<hr>

## Backend

### Creating files, writing boiler plate, and establishing connection to database

1. MongoDB Atlas
    - Create cluster for database
2. `npx create-react-app` inside project directory
3. make folder for your backend files
    - create a package.json file with `npm init -y`
    - install express, cors, mongoose, dotenv, nodemon
4. create a server.js file in your backend folder
    - require Express, cors, dotenv, mongoose, dotenv
    - `const app = express()`
        - initializes express with
    - `const port = process.env.PORT || 5000`
        - establish connection to port
    - `app.use(cors())`
    - `app.use(express())`
    - `app.listen(port,() => {`
        `console.log(server running on port ${port})`
        `})`
5. run `nodemon server.js` in terminal
    - to start server
6. establish URI and connection variables
    - create .env file to hold your ATLAS_URI connection string
    - include password in connection string
7. `mongoose.connect` and `connection.once` statements

### Writing Schemas - models folder

1. require mongoose
2. create a Schema variable equal to `mongoose.Schema`
3. create the Schema with the appropriate field criteria
4. create a variable equal to the name of the data equal to `mongoose.model`
5. export the schema

### Add API endpoint routes so the server can be used to perform the CRUD Operations - routes folder

1. Create the files in a routes folder 
    - i.e `exercises.js` `users.js`
2. Tell the server to use the files just created
    - require those files 
        - `const exerciseRouter = require("./routes/exercises")`
    - use those filesId
        - `app.use('/exercises', exerciseRouter)`
            - if someone goes to `/exercises` in the browser, it will display everything in the exerciseRouter
3. Inside the routes files
    - require the express router
        - `const router  = require('express').Router()`
    - require the model that is associated with this route
        - let User = require('../models/user.model');
4. establish routes associated with given model 
    1.  `router.router('/').get((req,res) => {`
            `User.find()`
                `.then(user => res.json(users))`
                `.catch(err => res.status(400).json("Error " + err))`
        `});`
        - `Users.find()`: mongoose method that will <b>get</b> (because its a get request) a list of users of all "users" in the database
            - the `find()` returns a promise
        - `.then(user => res.json(users))`: get all the users and return them in json format
        - `.catch` returns the error if there is one
    2.  `router.route('/add').post((req, res) => {`
            `const username = req.body.username;`
            `const newUser = new User({username});`
                `newUser.save()`
                    `.then(() => res.json('User added!'))`
                    `.catch(err => res.status(400).json('Error ' + err))`
        `});`
        - <b>post</b> request
        - `const username = req.body.username;`: takes data from the post request and formats it as a variable 
        - `const newUser = new User({username});` creates a new variable as a new User in the database
        - `newUser.save()` saves that user to the database
    3.  `router.route("/:id").get((req, res) => {`
            `Exercise.findById(req.params.id)`
                `.then((exercise) => res.json(exercise))`
                `.catch((err) => res.status(400).json("Error: " + err));`
        `});`
        - Finding (exercises) by id
            - `Exercise.findById(req.params.id)` is the mongoose query
    4.  `router.route("/:id").delete((req, res) => {`
            `Exercise.findByIdAndDelete(res.params.id)`
                `.then(() => res.json("Exercise deleted."))`
                `.catch((err) => res.status(400).json("Error: " + err));`
        `});`
        - Deleting specific data points with their id
        - `Exercise.findByIdAndDelete(res.params.id)` is the query for deleting with id
    5.  `router.route("update/:id").post((req, res) => {`
        `Exercise.findById(req.params.id)`
            `.then((exercise) => {`
                `exercise.username = req.body.username;`
                `exercise.description = req.body.description;`
                `exercise.duration = Number(req.body.duration);`
                `exercise.date = Date.parse(req.body.date);`
                `exercise.save()`
                    `.then(() => res.json("Exercise updated!"))`
                    `.catch((err) => res.status(400).json("Error: " + err));`
                `});`
            `.catch(err => res.status(400).json("Error: " + err));`
        `});`
        - updating specific data
        - 