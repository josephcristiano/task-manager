# Task manager application

## Setting up a MongoDB database
To start a new database instance run the following command
```sh
{path-to-mongod-file} --dbpath={path-to-data-storage}
mongodb/bin/mongod --dbpath=/Users/andresalba/mongodb-data
```

The connection URL would be: mongodb://127.0.0.1:27017

## Advanced postman summary
* Enviroment variables can be created to store data that is going to be used in all requests. This can be done by creating an environment (dev or prod) and then creating environment variables with key value pairs. Then variables can be used using the following syntax: `{{env_variable_name}}``

* Authorization can be added to all requests in a collection, by setting the authotization type in the *Authorization* tab for each request (default value is to be inherited for parent). Therefore, the authorization token must be defined for the whole collection. This can be done by selecting the collection options and then selecting *Edit*. The same options should appear but when a change is made it applies to all the requests.

* The authorization token can also be defined as an environment variable

* *Pre-requisite scripts* could be defined to run Javascript code before the request is sent. The same happens for the *Tests* tab, however this code is ran after sending the request.

* Sample script for defining an environment variable value
```js
// Check if the request was succesfull (pm object - short for postman)
if (pm.response.code === 200) {
    // Set the environment variable
    pm.environment.set('authToken', pm.response.json().token)
}
```

## MongoDB production instance configuration
* MongoDB hosting service must be used, the Atlas service is the one used in this course. This one is created by the MongoDB organization

* An account must be created at this [link](https://www.mongodb.com/cloud/atlas)

### Process for setting up a new instance (cluster)
1. Go to Clusters -> Create New Cluster
2. Select cloud provider, region, cluster tier, additional settings and cluster name
3. Once the instance is created, go to the Dashboard and click on *CONNECT*
4. Set up connection security
    1. Configure secure IP's. In this case all IP's must be whitelisted because heroku will continously change the server IP. To do so, the IP must be **0.0.0.0/0**.
    2. Create a database user
5. Connect to the database using the connection string defined in the GUI. Atlas instance connection client is Compass (Robo3T is not supported)

## Environment variables configuration in Heroku
* Set environment variable
```sh
heroku config:set {key}={value}
```
* View all environment variables
```sh
heroku config
```
* Remove an environment variable
```sh
heroku config:unset {key}
```