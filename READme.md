

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [Backend](#backend)
  - [Creating files, writing boiler plate, and establishing connection to database](#creating-files-writing-boiler-plate-and-establishing-connection-to-database)
  - [Writing Schemas - models folder](#writing-schemas-models-folder)
  - [Add API endpoint routes so the server can be used to perform the CRUD Operations - routes folder](#add-api-endpoint-routes-so-the-server-can-be-used-to-perform-the-crud-operations-routes-folder)
- [Frontend](#frontend)
  - [Setting up boilerplate, removing files if you so choose, and importing packages](#setting-up-boilerplate-removing-files-if-you-so-choose-and-importing-packages)
  - [Start building out your components](#start-building-out-your-components)
    - [Class vs Funcitonal components](#class-vs-funcitonal-components)
      - [Class Components](#class-components)
      - [Functional components](#functional-components)
  - [Connecting frontend to Backend](#connecting-frontend-to-backend)

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
        - updating specific data via the id in the url parameter
        - overwrite the data with the new input provided by the user
        - save it to the database via `exercise.save()`

<hr>

## Frontend

### Setting up boilerplate, removing files if you so choose, and importing packages

1. `import {BrowserRouter as Router, Routes, Route} from 'react-router-dom'` if you would like to use BrowserRouter
    - syntax for router components will look like this in the main app component vvv
    -   `<Router>`
            `<div className="container">`
                `<Navbar />`
                `<br />`
                `<Routes>`
                    `<Route path="/" exact element={<ExercisesList/>} />`
                    `<Route path="/edit/:id" element={<EditExercise/>} />`
                    `<Route path="/create" element={<CreateExercise/>} />`
                    `<Route path="/user" element={<CreateUser/>} />`
                `</Routes>`
            `</div>`
        `</Router>`
        - in V6 the syntax is different. You put everything in the App component inside the <b>Router</b>, but everything with a `<Route>` tag has to go inside another tag called `<Routes>`
        - instead of the tag being labeled "component", you now put your component titles in an "element" prop. You also have to wrap the component name is tags (< />) 
2. Remove react unnecessary boilerplate
3. import components that you plan to use in the App component at the top of the App.js file i.e:
    - `import Navbar from "./components/navbar.component";`

### Start building out your components 

1. Aside from the typical boiler plate, import link from 'react-router-dom' if youre using react-router-dom, this will allow us to link to differnt routes
    - `import { Link } from 'react-router-dom';` if you are using "Link" tags

#### Class vs Funcitonal components 
##### Class Components
- Class components are "stateful" meaning state can be used inside class based components
- Class components have a constructor function, this function is what is called to create the component on render 
    - constructor takes in (props) as its argument, as is where you declare your state as an object, i.e:
        - `this.state = { name: "", age: 100, isMale: true }`
        - this object declares your initial state for your key value pairs
    - also must include `super(props)` inside constructor before state if you want to pass (props) up to the parent constructor function
    - to change state you use `this.setState`
##### Functional components
- Functional components are "stateless" meaning in order to use state you need to use React hooks 

2. State is how you create variables in react
3. in order to change state you need to create methods inside the class component, above the render() method, and use the setState function inside that new method, i.e:
    -   `onChangeUsername(e) {`
            `this.setState({`
             `username: e.target.value,`
            `});`
        `}`
4. the <b>bind</b> keyword refers to binding the "this" inside each method to the class which it is refering (the class the method is inside of) 
    - without the `bind`, "this" would come back undefined
5. `componentDidMount` is a react lifestyle method, react automatically calls these at different points, "componentDidMount is called right before anything displays on the page

### Connecting frontend to Backend

1. `npm install axios`
    - axios allows frontend to send HTTP requests to the server endpoint on the backend 
    - link the axios request to the http endpoint that is related to what you are either posting to or recieving 
        - `axios.post('http://localhost:5000/users/add', user)`
            `.then(res => console.log(res.data))`
        - The "user", after the endpoint, is a json object sent to the server with the appropriate data