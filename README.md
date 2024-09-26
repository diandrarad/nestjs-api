<p align="center">
  <a href="http://nestjs.com/" target="blank"><img src="https://nestjs.com/img/logo-small.svg" width="120" alt="Nest Logo" /></a>
</p>

[circleci-image]: https://img.shields.io/circleci/build/github/nestjs/nest/master?token=abc123def456
[circleci-url]: https://circleci.com/gh/nestjs/nest

  <p align="center">A progressive <a href="http://nodejs.org" target="_blank">Node.js</a> framework for building efficient and scalable server-side applications.</p>
    <p align="center">
<a href="https://www.npmjs.com/~nestjscore" target="_blank"><img src="https://img.shields.io/npm/v/@nestjs/core.svg" alt="NPM Version" /></a>
<a href="https://www.npmjs.com/~nestjscore" target="_blank"><img src="https://img.shields.io/npm/l/@nestjs/core.svg" alt="Package License" /></a>
<a href="https://www.npmjs.com/~nestjscore" target="_blank"><img src="https://img.shields.io/npm/dm/@nestjs/common.svg" alt="NPM Downloads" /></a>
<a href="https://circleci.com/gh/nestjs/nest" target="_blank"><img src="https://img.shields.io/circleci/build/github/nestjs/nest/master" alt="CircleCI" /></a>
<a href="https://coveralls.io/github/nestjs/nest?branch=master" target="_blank"><img src="https://coveralls.io/repos/github/nestjs/nest/badge.svg?branch=master#9" alt="Coverage" /></a>
<a href="https://discord.gg/G7Qnnhy" target="_blank"><img src="https://img.shields.io/badge/discord-online-brightgreen.svg" alt="Discord"/></a>
<a href="https://opencollective.com/nest#backer" target="_blank"><img src="https://opencollective.com/nest/backers/badge.svg" alt="Backers on Open Collective" /></a>
<a href="https://opencollective.com/nest#sponsor" target="_blank"><img src="https://opencollective.com/nest/sponsors/badge.svg" alt="Sponsors on Open Collective" /></a>
  <a href="https://paypal.me/kamilmysliwiec" target="_blank"><img src="https://img.shields.io/badge/Donate-PayPal-ff3f59.svg" alt="Donate us"/></a>
    <a href="https://opencollective.com/nest#sponsor"  target="_blank"><img src="https://img.shields.io/badge/Support%20us-Open%20Collective-41B883.svg" alt="Support us"></a>
  <a href="https://twitter.com/nestframework" target="_blank"><img src="https://img.shields.io/twitter/follow/nestframework.svg?style=social&label=Follow" alt="Follow us on Twitter"></a>
</p>
  <!--[![Backers on Open Collective](https://opencollective.com/nest/backers/badge.svg)](https://opencollective.com/nest#backer)
  [![Sponsors on Open Collective](https://opencollective.com/nest/sponsors/badge.svg)](https://opencollective.com/nest#sponsor)-->

---

# API Documentation

## Authentication (Auth)

### 1. **Sign Up**
- **Endpoint**: `/auth/signup`
- **Method**: `POST`
- **Description**: Register a new user with email and password.
- **Request Body**:
  ```json
  {
    "email": "user@example.com",
    "password": "yourpassword"
  }
  ```
- **Response**: 
  - `201 Created`: Returns an access token.
    ```json
    {
      "access_token": "jwt_token_here"
    }
    ```
  - `403 Forbidden`: Credentials already taken (unique email constraint).
    ```json
    {
      "message": "Credentials taken"
    }
    ```
  - `400 Bad Request`: Validation failed for the request body.
  
### 2. **Sign In**
- **Endpoint**: `/auth/signin`
- **Method**: `POST`
- **Description**: Log in with email and password.
- **Request Body**:
  ```json
  {
    "email": "user@example.com",
    "password": "yourpassword"
  }
  ```
- **Response**: 
  - `200 OK`: Returns an access token.
    ```json
    {
      "access_token": "jwt_token_here"
    }
    ```
  - `403 Forbidden`: Invalid credentials (wrong email or password).
    ```json
    {
      "message": "Credentials incorrect"
    }
    ```

---

## User

### 1. **Get Me**
- **Endpoint**: `/user/me`
- **Method**: `GET`
- **Description**: Fetch the details of the logged-in user.
- **Headers**: 
  - `Authorization`: Bearer `<JWT Token>`
- **Response**: 
  - `200 OK`: Returns the user details.
    ```json
    {
      "id": 1,
      "email": "user@example.com",
      "firstName": "John",
      "lastName": "Doe"
    }
    ```
  - `401 Unauthorized`: Invalid or missing token.

### 2. **Edit User**
- **Endpoint**: `/user`
- **Method**: `PATCH`
- **Description**: Edit the logged-in user's information.
- **Headers**: 
  - `Authorization`: Bearer `<JWT Token>`
- **Request Body (Partial Update)**:
  ```json
  {
    "email": "new-email@example.com",
    "firstName": "John",
    "lastName": "Doe"
  }
  ```
- **Response**:
  - `200 OK`: User details updated, excluding the password hash.
    ```json
    {
      "id": 1,
      "email": "new-email@example.com",
      "firstName": "John",
      "lastName": "Doe"
    }
    ```
  - `401 Unauthorized`: Invalid or missing token.

---

## Bookmarks

### 1. **Get All Bookmarks**
- **Endpoint**: `/bookmark`
- **Method**: `GET`
- **Description**: Get all bookmarks for the logged-in user.
- **Headers**: 
  - `Authorization`: Bearer `<JWT Token>`
- **Response**:
  - `200 OK`: Returns an array of bookmarks.
    ```json
    [
      {
        "id": 1,
        "title": "Bookmark 1",
        "description": "Optional description",
        "link": "https://example.com"
      },
      {
        "id": 2,
        "title": "Bookmark 2",
        "description": null,
        "link": "https://example2.com"
      }
    ]
    ```
  - `401 Unauthorized`: Invalid or missing token.

### 2. **Get Bookmark by ID**
- **Endpoint**: `/bookmark/:id`
- **Method**: `GET`
- **Description**: Get a specific bookmark by ID.
- **Headers**: 
  - `Authorization`: Bearer `<JWT Token>`
- **Params**: 
  - `id`: Bookmark ID (integer).
- **Response**: 
  - `200 OK`: Returns the bookmark details.
    ```json
    {
      "id": 1,
      "title": "Bookmark Title",
      "description": "Optional description",
      "link": "https://example.com"
    }
    ```
  - `404 Not Found`: Bookmark not found.

### 3. **Create Bookmark**
- **Endpoint**: `/bookmark`
- **Method**: `POST`
- **Description**: Create a new bookmark.
- **Headers**: 
  - `Authorization`: Bearer `<JWT Token>`
- **Request Body**:
  ```json
  {
    "title": "Bookmark Title",
    "description": "Optional description",
    "link": "https://example.com"
  }
  ```
- **Response**: 
  - `201 Created`: Bookmark successfully created.
    ```json
    {
      "id": 1,
      "title": "Bookmark Title",
      "description": "Optional description",
      "link": "https://example.com"
    }
    ```
  - `400 Bad Request`: Validation failed.

### 4. **Edit Bookmark by ID**
- **Endpoint**: `/bookmark/:id`
- **Method**: `PATCH`
- **Description**: Edit an existing bookmark by ID.
- **Headers**: 
  - `Authorization`: Bearer `<JWT Token>`
- **Params**: 
  - `id`: Bookmark ID (integer).
- **Request Body (Partial Update)**:
  ```json
  {
    "title": "Updated Title",
    "description": "Updated Description",
    "link": "https://updated-example.com"
  }
  ```
- **Response**: 
  - `200 OK`: Bookmark updated successfully.
    ```json
    {
      "id": 1,
      "title": "Updated Title",
      "description": "Updated Description",
      "link": "https://updated-example.com"
    }
    ```
  - `403 Forbidden`: User does not own the bookmark or invalid access.
  - `404 Not Found`: Bookmark not found.

### 5. **Delete Bookmark by ID**
- **Endpoint**: `/bookmark/:id`
- **Method**: `DELETE`
- **Description**: Delete a bookmark by ID.
- **Headers**: 
  - `Authorization`: Bearer `<JWT Token>`
- **Params**: 
  - `id`: Bookmark ID (integer).
- **Response**: 
  - `204 No Content`: Bookmark successfully deleted.
  - `403 Forbidden`: User does not own the bookmark or invalid access.
  - `404 Not Found`: Bookmark not found.

---

## Project setup

```bash
$ npm install
```

## Compile and run the project

```bash
# development
$ npm run start

# watch mode
$ npm run start:dev

# production mode
$ npm run start:prod
```

## Run tests

```bash
# unit tests
$ npm run test

# e2e tests
$ npm run test:e2e

# test coverage
$ npm run test:cov
```

## Resources

Check out a few resources that may come in handy when working with NestJS:

- Visit the [NestJS Documentation](https://docs.nestjs.com) to learn more about the framework.
- For questions and support, please visit our [Discord channel](https://discord.gg/G7Qnnhy).
- To dive deeper and get more hands-on experience, check out our official video [courses](https://courses.nestjs.com/).
- Visualize your application graph and interact with the NestJS application in real-time using [NestJS Devtools](https://devtools.nestjs.com).
- Need help with your project (part-time to full-time)? Check out our official [enterprise support](https://enterprise.nestjs.com).
- To stay in the loop and get updates, follow us on [X](https://x.com/nestframework) and [LinkedIn](https://linkedin.com/company/nestjs).
- Looking for a job, or have a job to offer? Check out our official [Jobs board](https://jobs.nestjs.com).

## Support

Nest is an MIT-licensed open source project. It can grow thanks to the sponsors and support by the amazing backers. If you'd like to join them, please [read more here](https://docs.nestjs.com/support).

## Stay in touch

- Author - [Kamil Myśliwiec](https://twitter.com/kammysliwiec)
- Website - [https://nestjs.com](https://nestjs.com/)
- Twitter - [@nestframework](https://twitter.com/nestframework)

## License

Nest is [MIT licensed](https://github.com/nestjs/nest/blob/master/LICENSE).
