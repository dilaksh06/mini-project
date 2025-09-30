# MedBotanica API Documentation

---

## Common APIs

### 1. Register User

**Endpoint:**

```
POST {base-URL}/user/register
```

**Description:**  
Registers a new user, hashes the password, and stores the record in the database.

**Request Body:**

```json
{
  "name": "John Doe",
  "email": "johndoe@example.com",
  "password": "strongpassword123",
  "phone": "+911234567890"
}
```

**Response Body (Success):**

```json
{
  "message": "User registered successfully",
  "user": {
    "id": "64f1c123456789",
    "name": "John Doe",
    "email": "johndoe@example.com",
    "phone": "+911234567890",
    "created_at": "2025-09-30T12:34:56Z"
  }
}
```

**Response Body (Failure):**

```json
{
  "message": "Unable to register user"
}
```

---

### 2. Login User

**Endpoint:**

```
POST {base-URL}/user/login
```

**Description:**  
Logs in an existing user with email & password. If valid, generates a JWT token.

**Request Body:**

```json
{
  "email": "johndoe@example.com",
  "password": "strongpassword123"
}
```

**Response Body (Success):**

```json
{
  "message": "Login successful",
  "user": {
    "id": "64f1c123456789",
    "name": "John Doe",
    "email": "johndoe@example.com",
    "phone": "+911234567890",
    "created_at": "2025-09-30T12:34:56Z"
  },
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVC..."
}
```

**Response Body (Failure):**

```json
{
  "message": "Invalid credentials"
}
```

---

### 3. Logout User

**Endpoint:**

```
POST {base-URL}/user/logout
```

**Description:**  
Logs out the user by clearing the token on the client side.  
(_Note: Server does not store tokens; client is responsible for removing them._)

**Response:**

```json
{
  "message": "Logout successful"
}
```

---

## Prediction APIs

### 4. Image Captioning

**Endpoint:**

```
POST {base-URL}/prediction
```

**Description:**  
A logged-in user sends an image. The backend runs an image captioning model and returns a name/label with 5–6 possible captions.

**Request Body:**

```json
{
  "user_id": "64f1c123456789",
  "image": "<base64_encoded_image_or_file_upload>"
}
```

**Response Body:**

```json
{
  "message": "Prediction successful",
  "image_label": "Motorcyclist without helmet",
  "captions": [
    "A man riding a motorcycle",
    "The rider is not wearing a helmet",
    "A bike is on the road during the day",
    "Traffic visible in the background",
    "Potential safety violation detected",
    "Urban environment with vehicles"
  ]
}
```

---

## User Models

### User Document

```json
{
  "id": "64f1c123456789",
  "name": "John Doe",
  "email": "johndoe@example.com",
  "hashed_password": "bcrypt_hashed_password_here",
  "phone": "+911234567890",
  "created_at": "2025-09-30T12:34:56Z"
}
```

### Request Models

- **RegisterUser**
    

```json
{
  "name": "string",
  "email": "string",
  "password": "string (8–72 chars)",
  "phone": "string (optional)"
}
```

- **LoginUser**
    

```json
{
  "email": "string",
  "password": "string"
}
```

- **PredictionRequest**
    

```json
{
  "user_id": "string",
  "image": "base64 encoded image or file"
}
```

### Response Models

- **UserOut**
    

```json
{
  "id": "string",
  "name": "string",
  "email": "string",
  "phone": "string (optional)",
  "created_at": "datetime"
}
```

- **LoginSuccessResponse**
    

```json
{
  "message": "Login successful",
  "user": {UserOut},
  "token": "jwt_token"
}
```

- **PredictionResponse**
    

```json
{
  "message": "Prediction successful",
  "image_label": "string",
  "captions": ["string", "string", "string", "string", "string"]
}
```