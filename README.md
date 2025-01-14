# PrivateGPT API v1.3 Documentation

## Table of Contents
1. [**Overview**](#overview)
2. [**Authentication**](#authentication)
   - [Login](#login)
   - [Logout](#logout)
3. [**Chat Conversations**](#chat-management)
   - [Start a New Chat](#start-a-new-chat)
   - [Continue an Existing Chat](#continue-an-existing-chat)
   - [Get Information about an Existing Chat](#get-information-about-an-existing-chat)
4. [**Source Management**](#source-management)
   - [Store a New Source via Markdown](#add-a-new-source)
   - [Edit an Existing Source](#edit-an-existing-source)
   - [Delete a Source](#delete-a-source)
   - [Get All Sources for the Provided Group](#get-all-sources-for-the-provided-group)
5. [**Group Management**](#group-management)
   - [Get All Groups](#get-all-groups)
   - [Add a New Group](#add-a-new-group)
   - [Delete a Group](#delete-a-group)
6. [**User Management**](#user-management)
   - [Create a New User](#create-a-new-user)
   - [Edit an Existing User](#edit-an-existing-user)
   - [Delete a User](#delete-a-user)
7. [**List of Options**](#list-of-options)
   - [Language](#language)
   - [Roles](#roles)
8. [**Additional Notes**](#additional-notes)
9. [**License**](#license)

---

## Overview
The PrivateGPT API v1.3 provides a secure and structured way to interact with PrivateGPT's features. The key functionalities include managing authentication, chats, sources, groups, and users.

---

## Authentication

### Login
Get an access token for authentication in other API calls.
The user requesting a token needs to be existing beforehand within PrivateGPT.

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

Note: The `{user-email}` and `{user-password}` correspond to the credentials of an existing user in PrivateGPT.

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
Note: This token will be used in the following calls as {api-token}.

### Logout
Invalidate the API-Token

| **Method** | **Endpoint**                |
|------------|-----------------------------|
| DELETE     | `{base-url}/api/v1/logout`  |

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

## Chat Conversations

### Start a New Chat
Start a new chat. To use the LLMs ‘document knowledge’ (RAG), one or more available groups must be provided, or usePublic needs to be
true. If no groups are provided (or the list is empty) and usePublic is false the LLMs ‘general knowledge’ will be used instead.
Please find the available languages below under List of options.

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
Important: If `usePublic` is set to false and no groups are provided, RAG will be deactivated for this chat.


**Response:**
```json
{
  "data": {
    "chatId": "your-chat-id",
    "answer": "response-answer",
    "sources": [
                "source-id-1", 
                "source-id-2"
               ]
  },
  "message": "success",
  "status": 200
}
```
This `chatId` will be used in the continuing of a chat as {chatId}.

### Continue an Existing Chat
Continue an already started chat.

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

**Response:**
```json
{
  "data": {
    "chatId": "your-chat-id",
    "answer": "response-answer",
    "sources": [
                "source-id-1", 
                "source-id-2"
               ]
  },
  "message": "success",
  "status": 200
}
```

---

### Get Informations about an existing Chat
| **Method** | **Endpoint**                      |
|------------|-----------------------------------|
| GET        | `{base-url}/api/v1/chats/{chatId}`|

**Headers:**
- Accept: `application/json`
- Authorization: `Bearer {api-token}`

**Body:**
```json
{
  "question": "Your follow-up question?"
}
```

**Response:**
```json
{
    "data": {
        "chatId": "your-chat-id",
        "title": "My Chat",
        "language": "en",
        "groups": ["Group A", "Internals"],
        "messages": [
            {
                "question": "What is PrivateGPT?",
                "answer": "PrivateGPT is mindblowing!",
                "language": "en",
            }
        ]
    },
    "message": "success",
    "status": 200
}
```

---

## Source Management

### Store a New Source via Markdown
| **Method** | **Endpoint**                  |
|------------|-------------------------------|
| POST       | `{base-url}/api/v1/sources`   |

**Headers:**
- Accept: `application/json`
- Authorization: `Bearer {api-token}`

**Body:**
```json
{
  "name": "Source Title",
  "groups": ["Group A"], // optional
  "content": "Content in Markdown format"
}
```

**Response:**
```json
{
  "data": {
    "documentId": "new-source-id"
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
  "name": "My updated Title",
  "groups": ["Updated Group"], // optional, list may be empty if source should be public
  "content": "Updated content in Markdown format"
}
```
All Fields are optional and remain unchanged if not provided.

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

### Get all sources for the provided group
| **Method** | **Endpoint**                        |
|------------|-------------------------------------|
| POST       | `{base-url}/api/v1/sources/group`   |

**Headers:**
- Accept: `application/json`
- Authorization: `Bearer {api-token}`

**Body:**
```json
{
  "groupName": ["Group A"], // optional
}
```

**Response:**
```json
{
  "data": {
    "sourcees": [
            "<source-id 1>",
            "<source-id 2>",
            "<source-id 3>",
        ]
    },
  "message": "success",
  "status": 200
}
```
---

## Group Management

### Get All Groups
Get all available groups.
`personalGroups:`   all groups the current user can access. Usable for chat.
`assignableGroups:` all available groups. Usable for creating and editing a user and sources. If you're using PGPT MCP-Server, you can deny the access of lients to this option.

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
The identification of a user is in relation to the email address. Please find the different available roles please find them below under List of
options.

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
Note: All other fields than `e-mail` are //optional and remain unchanged if not provided.

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

# List of Options

## Language
Each language parameter corresponds to its language code and relate directly to those available in the chat of the web application.

- **en**: English
- **de**: Deutsch
- **it**: Italiano
- **fr**: Français
- **es**: Español
- **ar**: العربية
- **bg**: Български
- **cs**: Čeština
- **eo**: Esperanto
- **et**: Eesti
- **el**: Ελληνικά
- **fi**: Suomi
- **fa**: فارسی
- **he**: עברית
- **hi**: हिन्दी
- **hu**: Magyar
- **id**: Bahasa Indonesia
- **ja**: 日本語
- **lt**: Lietuvių
- **lv**: Latviešu
- **nl**: Nederlands
- **no**: Norsk
- **pl**: Polski
- **pt**: Português
- **pt-br**: Português (Brasil)
- **ro**: Română
- **ru**: Русский
- **sk**: Slovenčina
- **sv**: Svenska
- **tr**: Türkçe
- **uk**: Українська
- **zh**: 中文

---

## Roles
The roles are directly related to those available in the web application.

| Parameter              | Function                               |
|------------------------|----------------------------------------|
| **ad**                | Active Directory Settings             |
| **analytics**         | Analytics                             |
| **documents**         | Manage sources & upload files         |
| **confluence-documents** | Import data from Confluence          |
| **smtp**              | SMTP settings                         |
| **system**            | System logs and certificates          |
| **users**             | User administration                   |



## Additional Notes
- Ensure all requests use the appropriate `Authorization` header containing the API token.
- The API follows RESTful principles, and the payloads must adhere to JSON standards.
- For detailed error handling, refer to the API documentation.

---

## License
This project is licensed under the [MIT License](LICENSE).

---
