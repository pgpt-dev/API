# PrivateGPT API v1 Pre-Release Documentation

## Overview

This repository contains the pre-release definition of the PrivateGPT API version 1.3. The API enables secure interaction with PrivateGPT's features, including authentication, chat, source management, group management, and user management.

---

## Authentication

### Login
**Endpoint:**  
`POST {base-url}/api/v1/login`

**Headers:**
- Accept: `application/json`

**Body:**
```json
{
  "email": "{user-email}",
  "password": "{user-password}"
}
```

**Response:**
```json
{
  "data": {
    "token": "your-api-token"
  },
  "message": "success",
  "status": 200
}
```

**Description:**  
Obtain an API token for authenticating subsequent requests.

### Logout
**Endpoint:**  
`POST {base-url}/api/v1/logout`

**Headers:**
- Accept: `application/json`
- Authorization: `Bearer {api-token}`

**Response:**
```json
{
  "data": {},
  "message": "success",
  "status": 200
}
```

---

## Chat Management

### Start a New Chat
**Endpoint:**  
`POST {base-url}/api/v1/chats`

**Headers:**
- Accept: `application/json`
- Authorization: `Bearer {api-token}`

**Body:**
```json
{
  "language": "en",
  "question": "Hello World?",
  "usePublic": true,
  "groups": [] // optional
}
```

**Response:**
```json
{
  "data": {
    "chatId": "your-chat-id",
    "answer": "response-answer",
    "sources": ["source-id-1", "source-id-2"]
  },
  "message": "success",
  "status": 200
}
```

### Continue an Existing Chat
**Endpoint:**  
`POST {base-url}/api/v1/chats/{chatId}`

**Headers:**
- Accept: `application/json`
- Authorization: `Bearer {api-token}`

**Body:**
```json
{
  "question": "Your follow-up question?"
}
```

---

## Source Management

### Add a New Source
**Endpoint:**  
`POST {base-url}/api/v1/sources`

**Headers:**
- Accept: `application/json`
- Authorization: `Bearer {api-token}`

**Body:**
```json
{
  "title": "Source Title",
  "groups": ["Group A"], // optional
  "content": "Content in Markdown format"
}
```

**Response:**
```json
{
  "data": {
    "sourceId": "new-source-id"
  },
  "message": "success",
  "status": 200
}
```

### Edit an Existing Source
**Endpoint:**  
`PATCH {base-url}/api/v1/sources/{sourceId}`

**Headers:**
- Accept: `application/json`
- Authorization: `Bearer {api-token}`

---

## Group Management

### Get All Groups
**Endpoint:**  
`GET {base-url}/api/v1/groups`

**Headers:**
- Accept: `application/json`
- Authorization: `Bearer {api-token}`

**Response:**
```json
{
  "data": {
    "personalGroups": ["alpha", "beta"],
    "assignableGroups": ["alpha", "beta", "internal", "finance"]
  },
  "message": "success",
  "status": 200
}
```

### Add a New Group
**Endpoint:**  
`POST {base-url}/api/v1/groups`

**Headers:**
- Accept: `application/json`
- Authorization: `Bearer {api-token}`

**Body:**
```json
{
  "groupName": "Group A"
}
```

---

## User Management

### Create a New User
**Endpoint:**  
`POST {base-url}/api/v1/users`

**Headers:**
- Accept: `application/json`
- Authorization: `Bearer {api-token}`

**Body:**
```json
{
  "name": "User Name",
  "email": "user@example.com",
  "password": "UserPassword123",
  "language": "en", // optional, defaults to "en"
  "timezone": "UTC", // optional, defaults to "Europe/Berlin"
  "usePublic": true,
  "groups": ["Group A"],
  "roles": ["Sources"],
  "activateFtp": true,
  "ftpPassword": "FTPPassword!"
}
```

**Response:**
```json
{
  "data": {},
  "message": "success",
  "status": 200
}
```

### Delete a User
**Endpoint:**  
`DELETE {base-url}/api/v1/users`

**Headers:**
- Accept: `application/json`
- Authorization: `Bearer {api-token}`

**Body:**
```json
{
  "email": "user@example.com"
}
```

---

## Additional Notes

- Ensure all requests use the appropriate `Authorization` header containing the API token.
- The API follows RESTful principles, and the payloads must adhere to JSON standards.
- For detailed error handling, refer to the API documentation.

---

## License
This project is licensed under the [MIT License](LICENSE).

---
