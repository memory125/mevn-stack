# MEVN Stack

This project is intended to be used as a quickstarter for building a
[**M**ongo](https://www.mongodb.com/) [**E**xpress](https://expressjs.com/) [**V**ueJS](https://vuejs.org/) [**N**ode](https://nodejs.org/en/) stack. This is similar to a MEAN stack, except Angular has been swapped out for a VueJS single page application rendered on the client side.

This is also the code used in the second VueJS training at the UW-Parkside App Factory.

## Technologies
This project uses:

[Mongo](https://www.mongodb.com/) for a NoSQL database.

[Express](https://expressjs.com/) For an HTTP Server

[VueJS](https://vuejs.org/) For Views, with the [Vuetify](https://vuetifyjs.com/) Material Design Framework

[Node](https://nodejs.org/en/) For a JavaScript runtime

This project makes use of a logging utility I created called [trunks](https://github.com/aturingmachine/trunks).

## Installation

To install this project simply clone or download the repo:

`git clone https://github.com/aturingmachine/mevn-stack.git <dir name>`

`cd <dir name>`

`npm install`

`cp .env.example .env` then add in your local Mongo URI **Changing the PORT variable in the .env will require you to change it in the `views/config/http.js` file.**

### Setup/Development

To develop using this project you can run 

`npm run dev:serve`

and

`npm run dev:client` 

in seperate terminal instances. This will allow hot reloading of both changes to the server and changes to the client.

The server will require you to be running a local instance of [MongoDB](https://www.mongodb.com/).

`npm run static` will build the client-side JavaScript and start the hot reloading of the server environment. `npm run dev:serve` can also be used to just start the API if you are working on that prior to worrying about the client.

#### Scripts

A more detailed breakdown of the scripts are as follows:

| Command `npm run`| Server | Client |
| :------------- |:------------- |:- 
| `start`| Static| Static (requires `npm run build`)
| `dev:serve`      	| Hot reload | Static
| `dev:client` 		| None | Hot Reload 
| `build` | None | Bundled by Webpack
| `static` | Hot reload | Bundled by Webpack

### Project Structure

##### Backend

`/src`

`--/controllers/`-- Contains controllers for our API resources.

`--/database/`

`----/models/`-- Contains the models for our API Resources using [Mongoose](http://mongoosejs.com/).

`--/middleware/`-- Any middleware you may need can go here.

`--/routes/`-- All route definitions are here.

`----/api.js`-- Routes for the API.

`----/user.js`-- Routes specific to the user resource.

##### Frontend

`/views`

`--/config/http.js`-- Axios config for local request 

`--/pages/`-- Separate Component Pages go here.

`--/router/index.js`-- Config for [vue-router](https://github.com/vuejs/vue-router)

`--/App.vue`-- Component that has Nav-Drawer, Footer, and Toolbar wrapped around a router view of other components.

`--/main.js`-- Registers the Vue components and Router

`--/index.html`-- The file we return, has the Vue app in it.

### Requirements

This project will require:

* Node >=7.0

### Dependencies 

* Dependencies Via NPM
	* [Axios](https://github.com/axios/axios) For client side HTTP requests
	* [cors](https://github.com/expressjs/cors) For CORS during development
	* [dotenv](https://github.com/motdotla/dotenv) Loads our .env variables
	* [vue](https://vuejs.org/) Realtime data binding on the frontend
	* [vuetify](https://vuetifyjs.com/vuetify/quick-start) Material design for Vue
	* [vue-router](https://github.com/vuejs/vue-router) Router for the SPA 

### User Resource
The example resource is as follows

| Attribute     | Type         | Required|
| :-------------: |:-------------:| :-----:  |
| `name`      	| String 		| `true`  |
| `age`      	| Number        | `true`  |
| `email` 		| String        | `true`  |


### Existing Routes

All user endpoints are behind the `/api` endpoint.

#### `GET`
`/users` - returns a list of all users inside of an array called `data`.

`/users/:id` - where `:id` is the id of a `user` resource. The resource is then returned in JSON format.

#### `POST`
`/users` - Creates a new `user` resource based on the payload of the request.

#### `DELETE`
`/users/:id` - Delete a user resouce matching the `:id` specified.

#### `PUT`
`/users` - Update a user based on the payload of the request

##

The Client can be accessed by hitting the document root:

`localhost:8080/` Will send you to the application.


# Deployment Summary
## MongoDB Installation & Running
* Please refer to the [installation doc](https://docs.mongodb.com/manual/administration/install-on-linux/).
* Please refer to the [mongodb running doc](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/#run-mongodb-community-edition).
* command
	* `mkdir -p /data/db`
	* `chmod 775 -R /data/db`

## Runing This Repo
* `cd mevn-stack`
* `npm install`
* `npm run dev:serve` and `npm run dev:client`

## New Feature - Store more user data to MongoDB
* Currently, only one user can be stored because the constrait of MongoDB, when we want to create new user, it will prompt the following error:
 `MongoError: E1100 duplicate key error collection: mevn-stack.users index: username_1 dup key: {: null}`
* So we need to modify the user schema to the following formats
 `const definition = {`
   `name: {`
    `type: String,`
    `required: true`
   `},`
   `age: {`
     `type: Number,`
     `required: true`
   `},`
   `email: {`
     `type: String,`
     `required: true,`
     `index: {`
       `unique: true,`
       `sparse: true`
     `}`
   `}`
 `}`
 * [Mongoose doc](https://mongoosejs.com/docs/2.7.x/docs/indexes.html).
 * [StackoverFlow Doc](https://stackoverflow.com/questions/24430220/e11000-duplicate-key-error-index-in-mongodb-mongoose).