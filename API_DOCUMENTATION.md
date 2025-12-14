# API Documentation

Complete API reference for the Sweet Shop Management System.

## Base URL

```
http://localhost:3001/api
```

## Authentication

Most endpoints require JWT authentication. Include the token in the Authorization header:

```
Authorization: Bearer <your-jwt-token>
```

Tokens are obtained from the login or registration endpoints and are valid for 7 days.

---

## Authentication Endpoints

### Register User

Create a new user account.

**Endpoint:** `POST /auth/register`

**Authentication:** None required

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "password123",
  "full_name": "John Doe"  // Optional
}
```

**Validation:**
- `email`: Required, valid email format
- `password`: Required, minimum 6 characters
- `full_name`: Optional, string

**Success Response (201):**
```json
{
  "status": "success",
  "data": {
    "user": {
      "id": "uuid",
      "email": "user@example.com",
      "full_name": "John Doe",
      "role": "user"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

**Error Responses:**
- `400`: Missing required fields or validation error
- `400`: Email already exists

---

### Login User

Authenticate and receive a JWT token.

**Endpoint:** `POST /auth/login`

**Authentication:** None required

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

**Success Response (200):**
```json
{
  "status": "success",
  "data": {
    "user": {
      "id": "uuid",
      "email": "user@example.com",
      "full_name": "John Doe",
      "role": "user"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

**Error Responses:**
- `400`: Missing email or password
- `401`: Invalid credentials

---

### Get User Profile

Retrieve authenticated user's profile.

**Endpoint:** `GET /auth/profile`

**Authentication:** Required

**Success Response (200):**
```json
{
  "status": "success",
  "data": {
    "user": {
      "id": "uuid",
      "email": "user@example.com",
      "full_name": "John Doe",
      "role": "user",
      "created_at": "2024-01-15T10:30:00Z",
      "updated_at": "2024-01-15T10:30:00Z"
    }
  }
}
```

**Error Responses:**
- `401`: No token provided
- `403`: Invalid or expired token

---

## Sweets Management Endpoints

### Get All Sweets

Retrieve all sweets in the inventory.

**Endpoint:** `GET /sweets`

**Authentication:** None required

**Success Response (200):**
```json
{
  "status": "success",
  "data": {
    "sweets": [
      {
        "id": "uuid",
        "name": "Milk Chocolate Bar",
        "category": "chocolate",
        "description": "Smooth and creamy milk chocolate",
        "price": 2.50,
        "quantity": 100,
        "image_url": "https://example.com/image.jpg",
        "created_by": "uuid",
        "created_at": "2024-01-15T10:30:00Z",
        "updated_at": "2024-01-15T10:30:00Z"
      }
    ]
  }
}
```

---

### Search Sweets

Search and filter sweets by various criteria.

**Endpoint:** `GET /sweets/search`

**Authentication:** None required

**Query Parameters:**
- `name` (string, optional): Filter by name (case-insensitive partial match)
- `category` (string, optional): Filter by exact category
- `minPrice` (number, optional): Minimum price filter
- `maxPrice` (number, optional): Maximum price filter

**Example:**
```
GET /sweets/search?name=chocolate&minPrice=2&maxPrice=5
```

**Success Response (200):**
```json
{
  "status": "success",
  "data": {
    "sweets": [
      // Array of matching sweets
    ]
  }
}
```

---

### Get Sweet by ID

Retrieve details of a specific sweet.

**Endpoint:** `GET /sweets/:id`

**Authentication:** None required

**Success Response (200):**
```json
{
  "status": "success",
  "data": {
    "sweet": {
      "id": "uuid",
      "name": "Dark Chocolate Truffle",
      "category": "chocolate",
      "description": "Rich dark chocolate",
      "price": 3.50,
      "quantity": 50,
      "image_url": "https://example.com/image.jpg",
      "created_by": "uuid",
      "created_at": "2024-01-15T10:30:00Z",
      "updated_at": "2024-01-15T10:30:00Z"
    }
  }
}
```

**Error Responses:**
- `404`: Sweet not found

---

### Create Sweet

Add a new sweet to the inventory.

**Endpoint:** `POST /sweets`

**Authentication:** Required (any authenticated user)

**Request Body:**
```json
{
  "name": "New Sweet",
  "category": "candy",
  "description": "Delicious new candy",  // Optional
  "price": 1.99,
  "quantity": 200,
  "image_url": "https://example.com/image.jpg"  // Optional
}
```

**Validation:**
- `name`: Required, string
- `category`: Required, string
- `description`: Optional, string
- `price`: Required, number >= 0
- `quantity`: Required, integer >= 0
- `image_url`: Optional, string (URL)

**Success Response (201):**
```json
{
  "status": "success",
  "data": {
    "sweet": {
      "id": "uuid",
      "name": "New Sweet",
      "category": "candy",
      "description": "Delicious new candy",
      "price": 1.99,
      "quantity": 200,
      "image_url": "https://example.com/image.jpg",
      "created_by": "current-user-uuid",
      "created_at": "2024-01-15T10:30:00Z",
      "updated_at": "2024-01-15T10:30:00Z"
    }
  }
}
```

**Error Responses:**
- `400`: Missing required fields or validation error
- `401`: Not authenticated

---

### Update Sweet

Update details of an existing sweet.

**Endpoint:** `PUT /sweets/:id`

**Authentication:** Required (creator or admin only)

**Request Body:**
```json
{
  "name": "Updated Name",         // Optional
  "category": "chocolate",        // Optional
  "description": "New description", // Optional
  "price": 3.99,                  // Optional
  "quantity": 150,                // Optional
  "image_url": "https://..."      // Optional
}
```

**Note:** Only include fields you want to update.

**Success Response (200):**
```json
{
  "status": "success",
  "data": {
    "sweet": {
      // Updated sweet object
    }
  }
}
```

**Error Responses:**
- `400`: Validation error
- `401`: Not authenticated
- `403`: Not authorized (not creator or admin)
- `404`: Sweet not found

---

### Delete Sweet

Remove a sweet from the inventory.

**Endpoint:** `DELETE /sweets/:id`

**Authentication:** Required (admin only)

**Success Response (200):**
```json
{
  "status": "success",
  "message": "Sweet deleted successfully"
}
```

**Error Responses:**
- `401`: Not authenticated
- `403`: Not authorized (admin only)
- `500`: Delete failed

---

## Inventory Management Endpoints

### Purchase Sweet

Purchase a quantity of a sweet (decreases inventory).

**Endpoint:** `POST /sweets/:id/purchase`

**Authentication:** Required

**Request Body:**
```json
{
  "quantity": 2
}
```

**Validation:**
- `quantity`: Required, integer > 0
- Must not exceed available stock

**Success Response (200):**
```json
{
  "status": "success",
  "message": "Purchase successful",
  "data": {
    "purchase": {
      "id": "uuid",
      "sweet_id": "uuid",
      "user_id": "uuid",
      "quantity": 2,
      "total_price": 5.00,
      "created_at": "2024-01-15T10:30:00Z"
    },
    "remainingStock": 98
  }
}
```

**Error Responses:**
- `400`: Invalid quantity or insufficient stock
- `401`: Not authenticated
- `404`: Sweet not found

---

### Restock Sweet

Add quantity to a sweet's inventory.

**Endpoint:** `POST /sweets/:id/restock`

**Authentication:** Required (admin only)

**Request Body:**
```json
{
  "quantity": 50
}
```

**Validation:**
- `quantity`: Required, integer > 0

**Success Response (200):**
```json
{
  "status": "success",
  "message": "Restock successful",
  "data": {
    "sweet": {
      "id": "uuid",
      "name": "Chocolate Bar",
      "quantity": 150,  // Updated quantity
      // ... other fields
    }
  }
}
```

**Error Responses:**
- `400`: Invalid quantity
- `401`: Not authenticated
- `403`: Not authorized (admin only)
- `404`: Sweet not found

---

### Get Purchase History

Retrieve authenticated user's purchase history.

**Endpoint:** `GET /sweets/purchases`

**Authentication:** Required

**Success Response (200):**
```json
{
  "status": "success",
  "data": {
    "purchases": [
      {
        "id": "uuid",
        "sweet_id": "uuid",
        "user_id": "uuid",
        "quantity": 2,
        "total_price": 5.00,
        "created_at": "2024-01-15T10:30:00Z",
        "sweets": {
          "id": "uuid",
          "name": "Chocolate Bar",
          "category": "chocolate",
          "price": 2.50,
          // ... other sweet fields
        }
      }
    ]
  }
}
```

**Error Responses:**
- `401`: Not authenticated

---

## Error Response Format

All error responses follow this format:

```json
{
  "status": "error",
  "message": "Error description"
}
```

### Common Error Codes

- `400 Bad Request`: Invalid input or validation error
- `401 Unauthorized`: Authentication required but not provided
- `403 Forbidden`: Authenticated but not authorized for this action
- `404 Not Found`: Resource doesn't exist
- `500 Internal Server Error`: Server error

---

## Rate Limiting

Currently, there are no rate limits implemented. In production, consider:
- 100 requests per 15 minutes for authenticated users
- 20 requests per 15 minutes for unauthenticated users

---

## Testing the API

### Using cURL

**Register:**
```bash
curl -X POST http://localhost:3001/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"password123"}'
```

**Login:**
```bash
curl -X POST http://localhost:3001/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"password123"}'
```

**Get Sweets:**
```bash
curl http://localhost:3001/api/sweets
```

**Purchase Sweet (with auth):**
```bash
curl -X POST http://localhost:3001/api/sweets/{sweet-id}/purchase \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer {your-token}" \
  -d '{"quantity":2}'
```

### Using Postman

1. Import the API endpoints
2. Set base URL to `http://localhost:3001/api`
3. For authenticated requests:
   - Go to Authorization tab
   - Select "Bearer Token"
   - Paste your JWT token

---

## Database Schema Reference

### profiles
- `id` (uuid, primary key)
- `email` (text, unique)
- `full_name` (text)
- `role` (text: 'user' or 'admin')
- `created_at` (timestamptz)
- `updated_at` (timestamptz)

### sweets
- `id` (uuid, primary key)
- `name` (text)
- `category` (text)
- `description` (text)
- `price` (decimal(10,2))
- `quantity` (integer)
- `image_url` (text)
- `created_by` (uuid, foreign key)
- `created_at` (timestamptz)
- `updated_at` (timestamptz)

### purchase_history
- `id` (uuid, primary key)
- `sweet_id` (uuid, foreign key)
- `user_id` (uuid, foreign key)
- `quantity` (integer)
- `total_price` (decimal(10,2))
- `created_at` (timestamptz)

---

## Changelog

### v1.0.0 (Initial Release)
- User registration and authentication
- Sweet CRUD operations
- Search and filter functionality
- Purchase system
- Inventory management
- Purchase history tracking

---

For more information, see the main [README.md](README.md) file.
