**Secure Auth API** is a Node.js API built with Express, JWT, and Sequelize for secure authentication and authorization. It includes user registration, email activation, login, and token-based authentication with access and refresh tokens.

## Features

- **User Registration**: Register users with email activation.
- **Login**: Secure login using email and password.
- **JWT Authentication**: Uses JSON Web Tokens for secure authentication.
- **Access and Refresh Tokens**: Refresh token mechanism for seamless user sessions.
- **Error Handling**: Structured and consistent error handling.
- **CORS & Cookie Support**: Supports CORS and cookie-based authentication.

## Technologies Used

- **Node.js**
- **Express.js**
- **Sequelize** (ORM)
- **JWT** (JSON Web Tokens)
- **PostgreSQL** (Database)

## Environment Variables

The following environment variables need to be configured in the `.env` file for the application to run properly.

```plaintext
# Server Configuration
API_HOST=<your-api-host>
API_PORT=<your-api-port>

# PostgreSQL Database Configuration
POSTGRES_HOST=<your-db-host>
POSTGRES_DB=<your-db-name>
POSTGRES_USER=<your-db-username>
POSTGRES_PASSWORD=<your-db-password>

# SMTP Email Configuration (for account activation emails)
SMTP_HOST=<your-smtp-host>
SMTP_PORT=<your-smtp-port>
SMTP_USER=<your-smtp-username>
SMTP_PASSWORD=<your-smtp-password>

# JWT Secrets (used for signing access and refresh tokens)
JWT_SECRET=<your-jwt-secret>
JWT_SECRET_REFRESH=<your-jwt-refresh-secret>
```

### Notes:

- Replace each placeholder with the appropriate value for your environment.

## Endpoints:

### 1. **User Registration**

- **URL**: `/register`
- **Method**: `POST`
- **Description**: Register a new user.
- **Request Body**:
  ```json
  {
    "name": "John Doe",
    "email": "john@example.com",
    "password": "securePassword"
  }
  ```
- **Response**:
  ```json
  {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com",
    "activationToken": "b7f3c49e-fdd5-4be8-b4a2-065b1e24c5f2"
  }
  ```
- **Example**:

  ```bash
  curl -X POST http://{API_HOST}:{API_PORT}/register \
  -H "Content-Type: application/json" \
  -d '{"name": "John Doe", "email": "john@example.com", "password": "securePassword"}'
  ```

### 2. **Activate User**

- **URL**: `/activate/:activationToken`
- **Method**: `GET`
- **Description**: Activate a user's account using the activation token sent via email.
- **Response**:

  ```json
  {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com",
    "activationToken": null
  }
  ```

- **Example**:

  ```bash
  curl http://{API_HOST}:{API_PORT}/activate/b7f3c49e-fdd5-4be8-b4a2-065b1e24c5f2
  ```

### 3. **Login**

- **URL**: `/login`
- **Method**: `POST`
- **Description**: Login and receive an access token and refresh token.
- **Request Body**:
  ```json
  {
    "email": "john@example.com",
    "password": "securePassword"
  }
  ```
- **Response**:
  ```json
  {
    "id": 1,
    "email": "john@example.com",
    "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwiZW1haWwiOiJqb2huQGV4YW1wbGUuY29tIiwiaWF0IjoxNjI0MDIzMDM2LCJleHAiOjE2MjQwMjY2MzZ9.7uT8tn_d4wbKjdHQ9UckL4l4vO-4KCR5QfSM7ybToqA"
  }
  ```
- **Example**:

  ```bash
  curl -X POST http://{API_HOST}:{API_PORT}/login \
  -H "Content-Type: application/json" \
  -d '{"email": "john@example.com", "password": "securePassword"}'
  ```

### 4. **Refresh Token**

- **URL**: `/refresh`
- **Method**: `GET`
- **Description**: Refresh the access token using the refresh token stored in cookies.
- **Response**:

  ```json
  {
    "id": 1,
    "email": "john@example.com",
    "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MSwiZW1haWwiOiJqb2huQGV4YW1wbGUuY29tIiwiaWF0IjoxNjI0MDIzMDM2LCJleHAiOjE2MjQwMjY2MzZ9.7uT8tn_d4wbKjdHQ9UckL4l4vO-4KCR5QfSM7ybToqA"
  }
  ```

- **Example**:

  ```bash
  curl http://{API_HOST}:{API_PORT}/refresh --cookie "refreshToken={yourRefreshToken}"
  ```

### 5. **Logout**

- **URL**: `/logout`
- **Method**: `POST`
- **Description**: Logout the user and invalidate the refresh token.
- **Response**: No content (204 status code).
- **Example**:

  ```bash
  curl -X POST http://{API_HOST}:{API_PORT}/logout --cookie "refreshToken={yourRefreshToken}"
  ```

### 6. **Get All Users (Protected Route)**

- **URL**: `/users`
- **Method**: `GET`
- **Description**: Retrieve a list of all registered users (requires valid access token).
- **Headers**:
  ```bash
  Authorization: Bearer {accessToken}
  ```
- **Response**:

  ```json
  [
    {
      "id": 1,
      "name": "John Doe",
      "email": "john@example.com",
      "activationToken": null
    },
    {
      "id": 2,
      "name": "Jane Smith",
      "email": "jane@example.com",
      "activationToken": null
    }
  ]
  ```

- **Example**:

  ```bash
  curl http://{API_HOST}:{API_PORT}/users \
  -H "Authorization: Bearer {accessToken}"
  ```
