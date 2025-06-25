# Backend API

A Node.js Express backend application with user and admin authentication.

## Features

- User authentication and registration
- Admin authentication and management
- JWT token-based authentication
- Password hashing with bcrypt
- MongoDB database integration
- CORS enabled
- Request validation
- Token blacklisting for logout functionality

## Technology Stack

- **Node.js** - Runtime environment
- **Express.js** - Web framework
- **MongoDB** - Database
- **Mongoose** - ODM for MongoDB
- **JWT** - Authentication tokens
- **bcrypt** - Password hashing
- **express-validator** - Request validation
- **cors** - Cross-origin resource sharing
- **dotenv** - Environment variable management

## Project Structure

```
├── App.js                     # Express app configuration
├── Server.js                  # Server entry point
├── controller/                # Controllers for handling requests
│   ├── admin.controller.js    # Admin-related operations
│   └── user.controller.js     # User-related operations
├── db/                        # Database configuration
│   └── db.js                  # MongoDB connection
├── middleware/                # Custom middleware
│   └── auth.middleware.js     # Authentication middleware
├── models/                    # Database models
│   ├── admin.model.js         # Admin schema
│   ├── BlacklistToken.model.js # Token blacklist schema
│   └── user.model.js          # User schema
├── routes/                    # API routes
│   ├── admin.routes.js        # Admin routes
│   └── user.routes.js         # User routes
└── service/                   # Business logic layer
    ├── admin.service.js       # Admin services
    └── user.service.js        # User services
```

## Installation

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd Backend
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Create a `.env` file in the root directory and add the following environment variables:
   ```env
   PORT=3000
   MONGODB_URI=mongodb://localhost:27017/your-database-name
   JWT_SECRET=your-jwt-secret-key
   JWT_EXPIRES_IN=7d
   ```

4. Make sure MongoDB is running on your system.

## Running the Application

### Development Mode
```bash
npm run dev
```
This will start the server with nodemon for auto-reloading.

### Production Mode
```bash
npm start
```

The server will start on the port specified in your `.env` file or default to port 3000.

## API Endpoints

### User Endpoints

#### Register User
**POST** `/api/users/register`

**Request Body:**
```json
{
  "name": "John Doe",
  "email": "john.doe@example.com",
  "password": "securePassword123",
  "phone": "+1234567890"
}
```

**Response (201):**
```json
{
  "success": true,
  "message": "User registered successfully",
  "data": {
    "user": {
      "id": "64f1a2b3c4d5e6f7g8h9i0j1",
      "name": "John Doe",
      "email": "john.doe@example.com",
      "phone": "+1234567890",
      "createdAt": "2025-06-25T10:30:00.000Z"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

#### User Login
**POST** `/api/users/login`

**Request Body:**
```json
{
  "email": "john.doe@example.com",
  "password": "securePassword123"
}
```

**Response (200):**
```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "user": {
      "id": "64f1a2b3c4d5e6f7g8h9i0j1",
      "name": "John Doe",
      "email": "john.doe@example.com",
      "phone": "+1234567890"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

#### User Logout
**POST** `/api/users/logout`

**Headers:**
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Response (200):**
```json
{
  "success": true,
  "message": "Logout successful"
}
```

#### Get User Profile
**GET** `/api/users/profile`

**Headers:**
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Response (200):**
```json
{
  "success": true,
  "data": {
    "user": {
      "id": "64f1a2b3c4d5e6f7g8h9i0j1",
      "name": "John Doe",
      "email": "john.doe@example.com",
      "phone": "+1234567890",
      "createdAt": "2025-06-25T10:30:00.000Z",
      "updatedAt": "2025-06-25T10:30:00.000Z"
    }
  }
}
```

### Admin Endpoints

#### Register Admin
**POST** `/api/admin/register`

**Request Body:**
```json
{
  "name": "Admin User",
  "email": "admin@example.com",
  "password": "adminPassword123",
  "role": "admin"
}
```

**Response (201):**
```json
{
  "success": true,
  "message": "Admin registered successfully",
  "data": {
    "admin": {
      "id": "64f1a2b3c4d5e6f7g8h9i0j2",
      "name": "Admin User",
      "email": "admin@example.com",
      "role": "admin",
      "createdAt": "2025-06-25T10:30:00.000Z"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

#### Admin Login
**POST** `/api/admin/login`

**Request Body:**
```json
{
  "email": "admin@example.com",
  "password": "adminPassword123"
}
```

**Response (200):**
```json
{
  "success": true,
  "message": "Admin login successful",
  "data": {
    "admin": {
      "id": "64f1a2b3c4d5e6f7g8h9i0j2",
      "name": "Admin User",
      "email": "admin@example.com",
      "role": "admin"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

#### Admin Logout
**POST** `/api/admin/logout`

**Headers:**
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Response (200):**
```json
{
  "success": true,
  "message": "Admin logout successful"
}
```

#### Get All Users (Admin Only)
**GET** `/api/admin/users`

**Headers:**
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Query Parameters (Optional):**
```
?page=1&limit=10&search=john
```

**Response (200):**
```json
{
  "success": true,
  "data": {
    "users": [
      {
        "id": "64f1a2b3c4d5e6f7g8h9i0j1",
        "name": "John Doe",
        "email": "john.doe@example.com",
        "phone": "+1234567890",
        "createdAt": "2025-06-25T10:30:00.000Z",
        "isActive": true
      },
      {
        "id": "64f1a2b3c4d5e6f7g8h9i0j3",
        "name": "Jane Smith",
        "email": "jane.smith@example.com",
        "phone": "+1234567891",
        "createdAt": "2025-06-25T11:00:00.000Z",
        "isActive": true
      }
    ],
    "pagination": {
      "currentPage": 1,
      "totalPages": 5,
      "totalUsers": 50,
      "limit": 10
    }
  }
}
```

### Error Response Examples

**400 Bad Request:**
```json
{
  "success": false,
  "message": "Validation error",
  "errors": [
    {
      "field": "email",
      "message": "Please provide a valid email address"
    },
    {
      "field": "password",
      "message": "Password must be at least 6 characters long"
    }
  ]
}
```

**401 Unauthorized:**
```json
{
  "success": false,
  "message": "Invalid credentials"
}
```

**403 Forbidden:**
```json
{
  "success": false,
  "message": "Access denied. Admin privileges required."
}
```

**404 Not Found:**
```json
{
  "success": false,
  "message": "User not found"
}
```

**500 Internal Server Error:**
```json
{
  "success": false,
  "message": "Internal server error",
  "error": "Database connection failed"
}
```

## Authentication

This API uses JWT (JSON Web Tokens) for authentication. Include the token in the Authorization header:

```
Authorization: Bearer <your-jwt-token>
```

## Error Handling

The API returns appropriate HTTP status codes and error messages:

- `200` - Success
- `201` - Created
- `400` - Bad Request
- `401` - Unauthorized
- `403` - Forbidden
- `404` - Not Found
- `500` - Internal Server Error

## Security Features

- Password hashing using bcrypt
- JWT token authentication
- Token blacklisting for logout
- Request validation using express-validator
- CORS protection
- Environment variable protection

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-feature`)
3. Commit your changes (`git commit -am 'Add new feature'`)
4. Push to the branch (`git push origin feature/new-feature`)
5. Create a Pull Request

## License

This project is licensed under the ISC License.

## Support

For support or questions, please contact the development team or create an issue in the repository.
