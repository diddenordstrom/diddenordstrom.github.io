---
layout: post
title:  "express web app basics"
date:   2022-04-07 10:00:36 +0200
categories: tutorial
---

# express basics

This webapp will be built using the javascript framework express.
To make use of express we need to have npm installed.
Then create a project folder and go into that folder.
Here vi initialize the project by running npm init.
This will create a file named package.json which we need to modify to install dependencies for our project.
Add the code below into the package.json file. We will need to add some more stuff to this file later

```bash
"dependencies": {
    "express": "4.17.3"
}
```

Exit the file and run `npm i`, (short for `npm install`) and this will generate a node_modules folder and a package-lock.json, containg list of dependencies and packages. 
We do not touch these files as they are generated by npm.


# web application

Now we need to create the actual application. Create a folder called `src`  for all of our source code, then a .js file (often named index.js or app.js, I'll go with index.js) in the source folder. Paste the code below into it.

```js
const express = require("express")

const app = express()

app.use("/", function(req, res){
    res.json("test")
})

app.listen(8080, function(){
  console.log("express app listening on port 8080")
})
```

Start the app with `node src/index.js`.
If you go to your browser and type `localhost:8080` you should get a response with a json object containing "test".

This guide will make use of a three layered architecture, so we need to create the different subfolders for this.
In the src folder, create three different folders name _DAL_ _BLL _PL_, they stand for Data Access Layer, Business Logic Layer, and Presentation Layer.
More on that later, but for now move your index.js file into the PL folder.

To make our system easy to work with we should use docker to start everyhting for us.
Make sure docker is installed before proceeding.

Create two files in the root of your project, one ´Dockerfile´ and one ´docker-compose,yaml´
`docker-compose.yaml` will look like this:

```yaml
version: "3.8"
services:
  web-app:
    build: "./"
    ports: 
    - 8080:8080
    volumes:
    - ./src/:/src/
```

Dockerfile shall look like this:

```Dockerfile
FROM node:14.15.4
EXPOSE 8080
WORKDIR /
COPY package*.json ./
RUN npm install
COPY src src
CMD npm run start
```

Version numbers can differ, go to respective websights to find the correct version number.

_Dockerfile_ has a command to run at startup, `npm run start`. This command does not exists and we need to create it ourselves. Go back to _package.json_ and under "scripts", add the following line:

`"start": "nodemon src/PL/index.js -L"`

Here we are essentially adding a custom start script. We could add the command we used to start the app before, `node src/index.js`, but we want docker to restart automaticly when it detects any changes and for that we will make use of nodemon.
This means we are adding nodemon as a dependency and therefore under _depednecies_ in package.json we need to add the following line:

`"nodemon": "^2.0.7"`

The tree structure for your files should now look like this:

```
|__ docker-compose.yaml
|__ Dockerfile
|__ package.json
|__ package-lock.json
|__ src
    |__ BLL
    |__ DAL
    |__ PL
        |__ index.js

```

Install the packages with `npm i`.
Great, you web-app should now start if you run `docker-compose build && docker-compose up`
It should also listen to changes, test that by just adding some random text in index.js and save the file. 
You should see in the terminal window that nodemon recognized changes and restarted.


# routes

It is time to split our web app into different files.
We start by adding routes.
Routes are essentially a way of mounting functions to a specific url.

In your _PL_ folder, create a subfolder named _routes_. 
Go into the folder.
Letś create a router for users, so _users-router.js_ with the following code:

```js
const express = require("express")

module.exports = function(usersManager) {
    const router = express.Router()

    router.get("/", function(req, res){
        usersManager.getAllUsers(function(data){
            res.json(data)
        })
    })
    return router
}
```

Here we are creating a route that listens to "/".
It takes in the argument _userManager_, and responds with the data from the _userManager.getAllUsers()_ function.

This router will need to be mounted to work, so in our index.js file add the following code:

```js
const usersManager = require("../BLL/users-manager,js")
const usersRouter = require("./routes/users-router.js")

...

app.use("/users", usersRouter(usersManager))

...

app.listen(8080), function(){
    ...
})
```
The router is now mounted at "/users", but the object we are passing it _usersManager_ does not yet exists.
So add users-manager.js to your BLL folder and put the following code in it:

```js
const usersRepo = require( "../DAL/users-repo.js" )

function getAllUsers(callback){
    usersRepo.getAllUsers(callback)
}
module.exports = {
    getAllUsers
}
```
The BLL is responsible for any logic within our web-app, so if some calculations / conversions etc need to happen this is the place to do it.
The code fetches an object from DAL/users-repo.js and exports the function _getAllUsers_.
This means we need to go to _DAL_ and create the users-repo.js file with the following code:

```js
const users = [
    {
        id: "1",
        name: "foo",
        age: "bar"
    }, {
        id: "2",
        name: "foo",
        age: "bar"
    }
]
function getAllUsers(callback){
    callback(users)
}
module.exports = {
    getAllUsers
}
```

This file will eventually communicate with a database and fetch users from there, but for now we simply use data in memory as an array of users. 

Do you use see the workflow?
The Route is mounted and listening to a specific url. When a get request is done the router uses the BLL function getAllUsers, which fetches users from the DAL, which in turn fetches data from the Database.

Current folder strucutre:

```
|__ docker-compose.yaml
|__ Dockerfile
|__ package.json
|__ package-lock.json
|__ src
    |__ BLL
        |__ users-manager.js
    |__ DAL
        |__ users-repo.js
    |__ PL
        |__ index.js
        |__ routes
            |__ users-router.js

```

Now, localhost:8080/users shall return an array of two users :)








