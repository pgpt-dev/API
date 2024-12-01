
# PrivateGPT API v1 Pre-Release Documentation

## Table of Contents
1. [Overview](#overview)
2. [Authentication](#authentication)
   - [Login](#login)
   - [Logout](#logout)
3. [Chat Management](#chat-management)
   - [Start a New Chat](#start-a-new-chat)
   - [Continue an Existing Chat](#continue-an-existing-chat)
4. [Source Management](#source-management)
   - [Add a New Source](#add-a-new-source)
   - [Edit an Existing Source](#edit-an-existing-source)
   - [Delete a Source](#delete-a-source)
5. [Group Management](#group-management)
   - [Get All Groups](#get-all-groups)
   - [Add a New Group](#add-a-new-group)
   - [Delete a Group](#delete-a-group)
6. [User Management](#user-management)
   - [Create a New User](#create-a-new-user)
   - [Edit an Existing User](#edit-an-existing-user)
   - [Delete a User](#delete-a-user)
7. [Additional Notes](#additional-notes)
8. [License](#license)

---

## Overview

The PrivateGPT API v1.3 provides a secure and structured way to interact with PrivateGPT's features. The key functionalities include managing authentication, chats, sources, groups, and users.

---

## Authentication

### Login
| **Method** | **Endpoint**                |
|------------|-----------------------------|
| POST       | `{base-url}/api/v1/login`   |

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

### Logout
| **Method** | **Endpoint**                |
|------------|-----------------------------|
| POST       | `{base-url}/api/v1/logout`  |

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
| **Method** | **Endpoint**                |
|------------|-----------------------------|
| POST       | `{base-url}/api/v1/chats`   |

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
| **Method** | **Endpoint**                      |
|------------|-----------------------------------|
| POST       | `{base-url}/api/v1/chats/{chatId}`|

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
| **Method** | **Endpoint**                  |
|------------|-------------------------------|
| POST       | `{base-url}/api/v1/sources`   |

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
| **Method** | **Endpoint**                      |
|------------|-----------------------------------|
| PATCH      | `{base-url}/api/v1/sources/{sourceId}` |

**Headers:**
- Accept: `application/json`
- Authorization: `Bearer {api-token}`

**Body:**
```json
{
  "title": "Updated Title",
  "groups": ["Updated Group"], // optional
  "content": "Updated content in Markdown format"
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

### Delete a Source
| **Method** | **Endpoint**                      |
|------------|-----------------------------------|
| DELETE     | `{base-url}/api/v1/sources/{sourceId}`|

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

## Group Management

### Get All Groups
| **Method** | **Endpoint**                |
|------------|-----------------------------|
| GET        | `{base-url}/api/v1/groups`  |

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
| **Method** | **Endpoint**                |
|------------|-----------------------------|
| POST       | `{base-url}/api/v1/groups`  |

**Headers:**
- Accept: `application/json`
- Authorization: `Bearer {api-token}`

**Body:**
```json
{
  "groupName": "Group A"
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

### Delete a Group
| **Method** | **Endpoint**                |
|------------|-----------------------------|
| DELETE     | `{base-url}/api/v1/groups`  |

**Headers:**
- Accept: `application/json`
- Authorization: `Bearer {api-token}`

**Body:**
```json
{
  "groupName": "Group A"
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

---

## User Management

### Create a New User
| **Method** | **Endpoint**                |
|------------|-----------------------------|
| POST       | `{base-url}/api/v1/users`   |

**Headers:**
- Accept: `application/json`
- Authorization: `Bearer {api-token}`

**Body:**
```json
{
  "name": "User Name",
  "email": "user@example.com",
  "password": "UserPassword123",
  "language": "en", // optional
  "timezone": "UTC", // optional
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

### Edit an Existing User
| **Method** | **Endpoint**                      |
|------------|-----------------------------------|
| PATCH      | `{base-url}/api/v1/users`         |

**Headers:**
- Accept: `application/json`
- Authorization: `Bearer {api-token}`

**Body:**
```json
{
  "email": "user@example.com",
  "name": "Updated Name",
  "language": "en",
  "groups": ["Updated Group"]
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
| **Method** | **Endpoint**                |
|------------|-----------------------------|
| DELETE     | `{base-url}/api/v1/users`   |

**Headers:**
- Accept: `application/json`
- Authorization: `Bearer {api-token}`

**Body:**
```json
{
  "email": "user@example.com"
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

---

## Additional Notes

- Ensure all requests use the appropriate `Authorization` header containing the API token.
- The API follows RESTful principles, and the payloads must adhere to JSON standards.
- For detailed error handling, refer to the API documentation.

---

## License
This project is licensed under the [MIT License](LICENSE).

---
