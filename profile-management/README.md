# Authentication Microservice

This Docker image provides an authentication microservice with user management functionality. It allows users to sign up, sign in, sign out, and update their profile information. It uses JSON Web Tokens (JWT) for authentication.

```
docker run -p 3003:3003 -e MYSQL_HOST=<mysql_host> -e MYSQL_PORT=<mysql_port> -e MYSQL_USER=<mysql_user> -e MYSQL_PASSWORD=<mysql_password> -e MYSQL_DATABASE=<mysql_database> authentication-microservice
```

## Usage

To run a container from this image, use the following command:

```bash
docker run -p 3003:3003 authentication-microservice
```

The microservice will be accessible at `http://localhost:3003`.

### API Endpoints

- `POST /api/signup`: Sign up a new user.
- `POST /api/signin`: Sign in an existing user.
- `POST /api/signout`: Sign out the currently authenticated user.
- `GET /api/protected`: Example of a protected route that requires authentication.
- `PUT /api/update`: Update the profile information of the authenticated user.

## Environment Variables

This microservice does not require any environment variables to be set.

## Volumes

This microservice does not use any volumes.

## Building the Image

If you want to build the Docker image yourself, follow these steps:

1. Clone the repository containing the application code.
2. Navigate to the directory where the Dockerfile is located.
3. Run the following command to build the image:

```bash
docker build -t jyothsnav/authentication-microservice .
```

This command will build the Docker image using the provided Dockerfile and tag it as `authentication-microservice`.
