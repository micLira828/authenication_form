
# Authentication API Documentation

## Base URL
```
https://yourapi.com/api/v1
```

---

## 1. Register a Friend (Sign Up)
**POST** `/auth/register`

**Request Body** (JSON)
```json
{
  "first_name": "John",
  "last_name": "Doe",
  "email": "john@example.com",
  "phone": "123-456-7890",
  "password": "Password123!",
  "confirm_password": "Password123!"
}
```

**Response (201 Created)**
```json
{
  "id": "uuid",
  "first_name": "John",
  "last_name": "Doe",
  "email": "john@example.com",
  "phone": "123-456-7890",
  "created_at": "2026-02-07T15:00:00Z"
}
```

**Errors**
- 400 Bad Request – passwords don’t match or invalid input
- 409 Conflict – email already exists

---

## 2. Login
**POST** `/auth/login`

**Request Body**
```json
{
  "email": "john@example.com",
  "password": "Password123!"
}
```

**Response (200 OK)**
```json
{
  "token": "jwt-token-here",
  "friend": {
    "id": "uuid",
    "first_name": "John",
    "last_name": "Doe",
    "email": "john@example.com",
    "phone": "123-456-7890"
  }
}
```

**Errors**
- 401 Unauthorized – invalid credentials

---

## 3. Get Current Friend
**GET** `/auth/me`  
**Headers:** `Authorization: Bearer <jwt-token>`

**Response (200 OK)**
```json
{
  "id": "uuid",
  "first_name": "John",
  "last_name": "Doe",
  "email": "john@example.com",
  "phone": "123-456-7890"
}
```

**Errors**
- 401 Unauthorized – token missing or invalid

---

## 4. Update Profile
**PUT** `/auth/me`  
**Headers:** `Authorization: Bearer <jwt-token>`

**Request Body**
```json
{
  "first_name": "Jane",
  "last_name": "Doe",
  "phone": "987-654-3210"
}
```

**Response (200 OK)**
```json
{
  "id": "uuid",
  "first_name": "Jane",
  "last_name": "Doe",
  "email": "john@example.com",
  "phone": "987-654-3210",
  "updated_at": "2026-02-07T15:30:00Z"
}
```

---

## 5. Change Password
**PUT** `/auth/me/password`  
**Headers:** `Authorization: Bearer <jwt-token>`

**Request Body**
```json
{
  "current_password": "Password123!",
  "new_password": "NewPassword456!",
  "confirm_new_password": "NewPassword456!"
}
```

**Response (200 OK)** – Password updated successfully

**Errors**
- 400 Bad Request – current password wrong or new passwords don’t match

---

## 6. Logout
**POST** `/auth/logout`  
**Headers:** `Authorization: Bearer <jwt-token>`

**Response (200 OK)**
```json
{
  "message": "Logged out successfully"
}
```

**Errors**
- 401 Unauthorized – token missing or invalid

---

### Notes
- `confirm_password` fields are **frontend only**, never stored in the database.
- `password_hash` should be used to store hashed passwords.
- JWT tokens are used for authentication in protected routes.
