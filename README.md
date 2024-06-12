# Backend
This is the backend of the project. The frontend can be found [here](https://github.com/zishaxn/Auth-Project-Frontend).

## API Architecture

The API is built using different layers: routes, controllers, services, and models.

- **Routes**: Responsible for handling incoming requests and forwarding them to the appropriate controller.
- **Controllers**: Validate the request, call the appropriate service, and send back the response.
- **Services**: Handle the business logic, interact with the database, and any external services. Services may also call other services.
- **Models**: Interact with the database, containing the schema and any model utility methods.

\*\*\* For simple GET or DELETE requests that don't require any business logic, the controller may directly interact with the model.

### Error Handling

Errors are handled using a custom error handler middleware. The middleware catches all errors that occur in the application and processes them accordingly. Each controller needs to be wrapped with the `errorCatch()` utility function to ensure that any errors thrown within the controller are caught and passed on to the error handler middleware.

## Authentication Flow

When a user logs in, the server generates two JWTs: `AccessToken` and `RefreshToken`. Both JWTs are sent back to the client in secure, HTTP-only cookies. The `AccessToken` is short-lived (15 minutes) and is passed on EVERY request to authenticate the user. The `RefreshToken` is long-lived (30 days) and is ONLY sent to the `/refresh` endpoint to generate a new `AccessToken`, which will then be passed on subsequent requests.

The frontend has logic that checks for `401 AccessTokenExpired` errors. When this error occurs, the frontend sends a request to the `/refresh` endpoint to get a new `AccessToken`. If that returns a 200 (meaning a new `AccessToken` was issued), then the client will retry the original request, giving the user a seamless experience without having to log in again. If the `/refresh` endpoint errors, the user will be logged out and redirected to the login page.

## Run Locally

To get started, you need to have [Node.js](https://nodejs.org/en) installed. You also need to have MongoDB installed locally ([download here](https://www.mongodb.com/docs/manual/installation/)), or you can use a cloud service like [MongoDB Atlas](https://www.mongodb.com/atlas/database). You will also need to create a [Resend](https://resend.com) account to send emails.

### Clone the project

```bash
git clone https://github.com/nikitapryymak/mern-auth-jwt.git
```

### Go to the project directory

```bash
cd mern-auth-jwt
```

### Navigate to the backend directory

```bash
cd backend
```

### Use the right node version (using [nvm](https://github.com/nvm-sh/nvm))

```bash
nvm use
```

### Install Backend dependencies

```bash
npm install
```

Before running the server, you need to add your ENV variables. Create a `.env` file and use the `sample.env` file as a reference. For development, you can set the `EMAIL_SENDER` to a random string since the emails are sent with a resend sandbox account (when running locally).

```bash
cp sample.env .env
# open .env and add your variables
```

### Start the MongoDB server (if running locally)

```bash
# using homebrew
brew services start mongodb-community@7.0
```

### Start the API server

```bash
# runs on http://localhost:4004 (default)
npm run dev
```

### Postman Collection

There is a Postman collection in the `backend` directory that you can use to test the API. The `postman.json` contains requests for all the routes in the API. You can [import the JSON directly](https://learning.postman.com/docs/getting-started/importing-and-exporting/importing-data/#import-postman-data) into Postman.

## 🛠️ Build

To build the backend, run the following command in the backend directory:

```bash
npm run build
```

To test the compiled API code, run:

```bash
# this runs the compiled code in the dist directory
npm run start
```

### Acknowledgement
Thanks To [Nikita](https://github.com/nikitapryymak) For This Amazing Tutorial.
