# Document Management System API

[![Build Status](https://travis-ci.org/andela-venogwe/docman.svg?branch=staging)](https://travis-ci.org/andela-venogwe/docman)
[![Code Climate](https://codeclimate.com/repos/58aec2c282269f0292000169/badges/7f69e0e8445c725eecd8/gpa.svg)](https://codeclimate.com/repos/58aec2c282269f0292000169/feed)
[![Issue Count](https://codeclimate.com/repos/58aec2c282269f0292000169/badges/7f69e0e8445c725eecd8/issue_count.svg)](https://codeclimate.com/repos/58aec2c282269f0292000169/feed)
[![Coverage Status](https://coveralls.io/repos/github/andela-venogwe/docman/badge.png?branch=staging)](https://coveralls.io/github/andela-venogwe/docman?branch=staging)

Document Management System provides a restful API for users to create and manage documents giving different privileges based on user roles and managing authentication using JWT. The API has routes, each dedicated to a single task that uses HTTP response codes to indicate API status and errors.

---

## *Features*

### Authentication

Users are assigned a token when signup or signin. This token is needed for subsequent HTTP requests to the API for authentication and can be attached as values to the header's `x-acess-token` key. API requests made without authentication will fail with the status code of `401`.

- It uses JWT for authentication.
- It generates a token and returns to the client.
- It verifies the token on every request to authenticated endpoints.

### Users

- It allows users to be created.
- It sets a newly created user's roleÌd to `1`(a regular user) by default.
- Admin users are assigned a roleId of `0`.
- It allows the created user and the admin to edit, and update thier information.
- All registered users can be retrieved by the admin user.

### Roles

- It ensures that users have a role.
- It ensures users roleId could be `0`(admin) or `1`(regular user).
- It allows an admin to update a user's role.

### Documents

- It allows new documents to be created/saved by users.
- It ensures all documents have an access defined (default access is `private`).
- It allows only admin users to retrieve all documents regardless of the document access.
- It ensures that only the user can retrieve their private documents.
- It ensures only authenticated users can delete, edit and update documents they own.
- It allows admin to delete any document regardless of the document access level.

---

## API Endpoints And Functions

EndPoints                                                   |   Functionality
------------------------------------------------------------|------------------------
POST /login                                                 |   Logs a user in.
POST /logout                                                |   Logs a user out.
POST /api/v1/users/                                         |   Creates a new user.
GET /api/v1/users/                                          |   Find matching instances of user.
GET /api/v1/users/:id                                       |   Find user.
PATCH /api/v1/users/:id                                     |   Update user attributes.
DELETE /api/v1/users/:id                                    |   Delete user.
POST /api/v1/documents/                                     |   Creates a new document instance.
GET /api/v1/documents/                                      |   Find matching instances of document.
GET /api/v1/documents?offset=:offset                        |   Find matching instances of document with an `offset`.
GET /api/v1/documents?limit=:limit                          |   Find matching instances of document with a `limit` between `1` and `10`
GET /api/v1/documents/:id                                   |   Find document.
PATCH /api/v1/documents/:id                                 |   Update document attributes.
DELETE /api/v1/documents/:id                                |   Delete document.
GET /api/v1/documents/user/:id                              |   Find all documents belonging to the user.
GET /api/v1/search/users/:user                              |   Gets all users with usernames or emails matching the search `user` query
GET /api/v1/search/documents?phrase=:phrase                 |   Get all documents with title or excerpt containing the search `phrase`
GET /api/v1/search/documents?phrase=:phrase&access=:access  |   Get all document owned or accessible by `userId` with title containing the search `phrase`

---

## Sample API Request And Response

### Users EndPoint

#### Create User

##### Create User Request Body

- Endpoint: POST: `api/v1/users`
- Body `(application/json)`

```json
{
  "username": "uniqueuser",
  "firstname": "Unique User",
  "lastname": "lastname",
  "email": "uniqueuser@unique.com",
  "RoleId": 1,
  "password": "password",
  "password_confirmation": "password"
}
```

##### Create User Response Body

- Status: `200`

- Body `(application/json)`

```json
{
  "success": true,
  "message": {
    "id": 141,
    "username": "uniqueuser",
    "firstname": "Unique User",
    "lastname": "lastname",
    "email": "uniqueuser@unique.com",
    "roleId": 1
  }
}
```

#### Get Users

##### Get Users Request Body

- Endpoint: GET: `api/v1/users`
- Requires: Authentication, Admin Role

##### Get Users Response Body

- Status: `200`
- Body `(application/json)`

```json
[{
  "id": 140,
  "username": "username",
  "firstname": "firstname",
  "lastname": "lastname",
  "email": "email@host.com",
  "roleId": 1
},
{
  "id": 141,
  "username": "uniqueuser",
  "firstname": "firstname",
  "lastname": "hello",
  "email": "uniqueuser@unique.com",
  "roleId": 1
}]
```

### Documents Endpoint

#### Get All Documents

##### Get All Documents Request Body

- Endpoint: GET: `/api/v1/documents`

- Requires: Authentication, Admin Role

##### Get All DOcuments Response Body

- Status: `200`
- Body `(application/json)`

```json
[{
    "id": 45,
    "title": "Another new document",
    "content": "Test Epic things like lorem etc",
    "excerpt": "Test Epic things like lorem etc",
    "access": "public",
    "creatorId": 29,
    "createdAt": "2017-02-17T17:40:45.146Z",
    "updatedAt": "2017-02-17T17:40:45.146Z"
  },
  {
    "id": 44,
    "title": "New Title",
    "content": "The unique content of a document does not lie in the presence of the word unique",
    "excerpt": "Test Epic things like lorem etc",
    "access": "private",
    "creatorId": 1,
    "createdAt": "2017-02-06T22:55:43.747Z",
    "updatedAt": "2017-02-06T22:55:43.747Z"
  }]
```

#### Create Document

##### Create Document Request Body

- Endpoint: POST: `/api/v1/documents`
- Requires: Authentication
- Body `(application/json)`

```json
{
  "title": "Just a Title",
  "excerpt": "Test Epic things like lorem etc",
  "content": "This placeholder should not always be a lorem generated document",
  "creatorId": 1,
  "access": "user"
}
```

##### Create Document Response Body

- Status: `201: Created`
- Body `(application/json)`

```json
{
  "id": 1,
  "title": "Just a Title",
  "excerpt": "Test Epic things like lorem etc",
  "content": "This placeholder should not always be a lorem ipsum generated document",
  "creatorId": 1,
  "access": "user",
  "createdAt": "2017-02-05T05:51:51.217Z",
  "updatedAt": "2016-02-05T05:51:51.217Z"
}
```

#### Get Document

##### Get Docucment Request Body

- Endpoint: GET: `/api/v1/documents/:id`

- Requires: Authentication

##### Get Document Response Body

- Status: `200: OK`
- Body `(application/json)`

```json
{
  "id": 1,
  "title": "Just a Title",
  "excerpt": "Test Epic things like lorem etc",
  "content": "This placeholder should not always be a lorem ipsum generated document",
  "creatorId": 1,
  "access": "private",
  "createdAt": "2017-02-05T05:51:51.217Z",
  "updatedAt": "2016-02-05T05:51:51.217Z"
}
```

#### Edit Document

##### Edit Document Request Body

- Endpoint: PATCH: `/api/v1/documents/:id`
- Requires: Authentication
- Body `(application/json)`:

```json
{
  "title": "Updated Title",
}
```

##### Edit Document Response Body

- Status: `200`
- Body `(application/json)`

```json
  {
    "id": 1,
    "title": "Updated Title",
    "excerpt": "Test Epic things like lorem etc",
    "content": "This placeholder should not always be a lorem ipsum generated document",
    "creatorId": 1,
    "access": "private",
    "createdAt": "2017-02-05T05:51:51.217Z",
    "updatedAt": "2016-02-05T05:51:51.217Z"
  }
```

#### Delete Document

##### Delete Document Request Body

- Endpoint: DELETE: `/api/v1/documents/:id`
- Requires: Authentication

##### Delete Document Response Body

- Status: `200`

- Body `(application/json)`

```json
{ "success": true,
  "message": "Document with id:42 deleted"
}
```

### Search

#### Documents Search

##### Documents Search Request Body

- Endpoint: GET: `/api/v1/search/documents?phrase=:phrase`

- Requires: Authentication

##### Documents Search Response Body

- Status: `200: OK`
- Body `(application/json)`

```json
[{
    "id": 45,
    "title": "Another new document",
    "content": "Test Epic things like lorem etc",
    "excerpt": "Test Epic things like lorem etc",
    "access": "public",
    "creatorId": 29,
    "createdAt": "2017-02-17T17:40:45.146Z",
    "updatedAt": "2017-02-17T17:40:45.146Z"
  },
  {
    "id": 44,
    "title": "New Title",
    "excerpt": "Test Epic things like lorem etc",
    "content": "The unique content of a document does not lie in the presence of the word unique",
    "access": "1",
    "creatorId": 1,
    "createdAt": "2017-02-06T22:55:43.747Z",
    "updatedAt": "2017-02-06T22:55:43.747Z"
  }]
```

#### Users Search

##### Users Search Request Body

- Endpoint: GET: `/api/v1/search/users?user:user`
- Requires: Authentication, Admin Role

##### Users Search Response Body

- Status: `200: OK`
- Body `(application/json)`

```json
[{
  "id": 140,
  "username": "username",
  "firstname": "firstname",
  "lastname": "lastname",
  "email": "email@host.com",
  "roleId": 1,
  "createdAt": "2017-02-17T19:41:30.837Z",
  "updatedAt": "2017-02-17T19:41:30.837Z"
},
{
  "id": 141,
  "username": "uniqueuser",
  "firstname": "teller",
  "lastname": "soloing",
  "email": "uniqueuser@unique.com",
  "roleId": 1,
  "createdAt": "2017-02-19T17:34:19.992Z",
  "updatedAt": "2017-02-19T17:34:19.992Z"
}]
```

---

## Technologies Used

- JavaScript (ES6)
- Node.js
- Express
- Mysql
- Sequelize ORM.

---

## Local Development

### Prerequisite Installations

- [Msql](https://www.mysql.com/) and
- [Node.js](http://nodejs.org/) >= v6.8.0.

### Procedure

1. Clone this repository from a terminal `git clone git@github.com:andela-venogwe/docman.git`.
1. Move into the project directory `cd docman`
1. Install project dependencies `npm install`
1. Start the express server `npm start`.
1. Run test `npm test`.

---

## Postman Collection

[![Run in Postman](https://run.pstmn.io/button.svg)](https://www.getpostman.com/collections/93ff8d3afef2a485f06d)

Create a Postman environment and set `url` and `token` variables.

---

## Deployment

Deploy this project to Heroku by clicking the button below.

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/andela-venogwe/docman/tree/staging)

Set the following environment variables below according to your node environment.

### Environment variables

- `PORT` sets the port to run the app
- `DB_NAME` sets the database name
- `DB_HOST` sets the database host
- `DB_TYPE` sets the database type eg `mysql`
- `DB_PORT` sets the  database connection port
- `DB_USER` sets the database user
- `DB_PASS` sets the  database password
- `NODE_ENV` sets the node environment to run the app.
- `JWT_SECRET` sets your json web token secret
- `AUTH_TOKEN` sets the authentication token
- `ADMIN_USERNAME` sets the admin username
- `ADMIN_PASSWORD` sets the admin password
- `ADMIN_FIRSTNAME` sets the admin firstname
- `ADMIN_LASTNAME` sets the admin lastname
- `ADMIN_EMAIL` sets the admin email address