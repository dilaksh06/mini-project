Got it ✅ — here’s the **fully updated `README.md`** file for your **MedBotanica API**, now matching the latest backend design for the **caption (prediction)** endpoint, including JWT header authentication and improved response structure.

---

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
Logs in an existing user with email & password.  
If valid, generates a **JWT token** that must be used for protected routes.

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
POST {base-URL}/user/prediction
```

**Authentication Required:** ✅ Yes (JWT Token)

**Headers:**

```
Authorization: Bearer <jwt_token>
Content-Type: multipart/form-data
```

**Description:**  
A logged-in user uploads an image.  
The backend authenticates the user using the token, processes the image through the **image captioning model**, and returns:

- The **predicted label** (e.g., herbal plant name)
    
- **Top captions** generated for the image
    
- Model metadata such as confidence and version
    

---

### 🧠 Example Request (Form Data)

|Key|Type|Description|
|---|---|---|
|`image`|File|Image file to caption (e.g., `.jpg`, `.png`)|

**Example using Axios (TypeScript / React):**

```tsx
const formData = new FormData();
formData.append("image", selectedFile);

const response = await axios.post(
  `${BASE_URL}/user/prediction`,
  formData,
  {
    headers: {
      Authorization: `Bearer ${token}`,
      "Content-Type": "multipart/form-data"
    }
  }
);

console.log(response.data);
```

---

**Response Body (Success):**

```json
{
  "message": "Prediction successful",
  "data": {
    "image_label": "Ocimum tenuiflorum (Tulsi)",
    "captions": [
      "A green herbal plant with oval leaves",
      "This looks like holy basil growing in a pot",
      "Close-up of Tulsi plant leaves",
      "An herbal plant used for medicinal purposes",
      "A small plant with green leaves and stem"
    ],
    "confidence": 0.94,
    "model_version": "v1.0.2"
  }
}
```

**Response Body (Failure - Invalid Token):**

```json
{
  "message": "Invalid or expired token"
}
```

**Response Body (Failure - Model Error):**

```json
{
  "message": "Prediction failed. Please try again later."
}
```

---

## 🧩 User Models

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

---

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

- **PredictionRequest (FormData or base64)**  
    _(Sent via multipart/form-data or as JSON)_
    

```json
{
  "image": "base64 encoded image or file"
}
```

---

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
  "data": {
    "image_label": "string",
    "captions": ["string", "string", "string", "string", "string"],
    "confidence": "float",
    "model_version": "string"
  }
}
```

---

## 🔐 Authentication Notes

- All authenticated routes require a **JWT** in the header.
    
- Format:
    
    ```
    Authorization: Bearer <jwt_token>
    ```
    
- Tokens expire after a predefined duration (configured in backend).
    
- Invalid or expired tokens return:
    
    ```json
    { "message": "Invalid or expired token" }
    ```
    

---

Would you like me to add an **extra example section** showing how to send the image request using **Fetch API** too (besides Axios)? It helps if you plan to document frontend integration in this README.