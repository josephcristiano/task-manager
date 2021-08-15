# Task manager application

## Config
The application expects the following environment variables:

```
PORT
MONGODB_URL
JWT_SECRET
FROM_EMAIL
SENDGRID_API_KEY
```

The develompent, and test environment variables should be placed into the following files:
 * Development - `/config/dev.env`
 * Test - `/config/test.env`

## Endpoints
All endpoints that accepts `POST` and `PATCH` request methods, expect `application/json` content type.

\* - Requires a valid JWT token as an HTTP request header (`Authorization: Bearer <jwt_token>`), which is sent from the authorization endpoints in the response body.

* Authorization
  * Create user                     - `POST /users`
  * Login user                      - `POST /users/login`
* User actions *
  * Logout user                     - `POST /users/logout`
  * Logout all users                - `POST /users/logout-all`
  * Read profile                    - `GET /users/me`
  * Update user                     - `PATCH /users/me`
  * Delete user                     - `DELETE /users/me`
  * Upload avatar                   - `POST /users/me/avatar`
  * Delete avatar                   - `DELETE /users/me/avatar`
  * Get user avatar                 - `GET /users/:id/avatar`
* Task management *
  * Create task                     - `POST /tasks`
  * Read tasks                      - `GET /tasks`
    * completed       - `Boolean`
    * sortBy          - `<field_name>:asc|desc`
    * limit           - `Number`
    * skip            - `Number`
  * Read task                       - `GET /tasks/:id`
  * Update task                     - `PATCH /tasks/:id`
  * Delete task                     - `DELETE /tasks/:id`

### Example route (Create user)
----
  Creates a new user in the database.
* **URL**

  `/users`

* **Method:**

  `POST`

* **Data Params**

   **Required:**
 
   * `name=[string]`
   * `email=[string]` - Must be unique in the users collection
   * `password=[string]` - Minimum length is 7, and cannot contain the word: *password*

   **Optional:**
 
   `age=[number]` - Defaults to 0

* **Success Response:**

  * **Code:** 201 Created <br />
  * **Content:** <br />
    ```
    {
      "user": {
          "age": 43,
          "_id": "5c88d692da13be0017bda5e1",
          "name": "John",
          "email": "john@doe.com",
          "createdAt": "2019-03-13T10:08:18.045Z",
          "updatedAt": "2019-03-13T10:08:18.226Z",
          "__v": 1
      },
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI1Yzg4ZDY5MmRhMTNiZTAwMTdiZGE1ZTEiLCJpYXQiOjE1NTI0NzE2OTh9.mLgW3hHi5vOgexwOYYkPZSNP0oaFGTXLZJSFpZlStzA"
    }
    ```
 
* **Error Response:**

  * **Code:** 400 Bad Request <br />


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
