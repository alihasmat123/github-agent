 API Reference Guide

This document defines the guidelines, conventions, and specifications for the APIs exposed by this application.

## Base URL

In development and production environments, the API is accessible at the following base URLs:

- **Local Development:** `http://localhost:8080/api/v1`
- **Staging:** `https://staging.example.com/api/v1`
- **Production:** `https://api.example.com/api/v1`

## Design Conventions

- **Protocol:** HTTPS (strictly enforced in production).
- **Format:** Request and response bodies must be in JSON (`application/json`).
- **Standard HTTP Methods:**
  - `GET`: Retrieve a resource or a list of resources.
  - `POST`: Create a new resource.
  - `PUT`: Update/replace an existing resource.
  - `PATCH`: Partially update an existing resource.
  - `DELETE`: Delete a resource.

## Standard Response Structure

All responses return a structured JSON object containing consistent fields.

### Success Response (HTTP 200/201)

```json
{
  "success": true,
  "data": {
    "id": "123e4567-e89b-12d3-a456-426614174000",
    "name": "Sample Item",
    "createdAt": "2023-10-01T12:00:00Z"
  },
  "metadata": {
    "timestamp": "2023-10-01T12:00:01Z"
  }
}
```

### Error Response (HTTP 4xx/5xx)

```json
{
  "success": false,
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "The requested item could not be found.",
    "details": {
      "id": "123e4567-e89b-12d3-a456-426614174000"
    }
  },
  "metadata": {
    "timestamp": "2023-10-01T12:00:01Z"
  }
}
```

## Core Endpoints

### Authentication

#### Post Login
- **Endpoint:** `POST /auth/login`
- **Description:** Authenticates a user and returns a JSON Web Token (JWT).
- **Request Body:**
  ```json
  {
    "email": "user@example.com",
    "password": "securepassword"
  }
  ```
- **Success Response (200 OK):**
  ```json
  {
    "success": true,
    "data": {
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
      "expiresIn": 3600
    }
  }
  ```

---

### Resources

#### List Resources
- **Endpoint:** `GET /resources`
- **Description:** Fetches a paginated list of resources.
- **Query Parameters:**
  - `page` (optional): Page number (default: 1)
  - `limit` (optional): Number of records per page (default: 10, max: 100)
- **Success Response (200 OK):**
  ```json
  {
    "success": true,
    "data": [
      {
        "id": "1",
        "name": "Resource A"
      }
    ],
    "metadata": {
      "page": 1,
      "limit": 10,
      "totalCount": 42
    }
  }
  ```

#### Create Resource
- **Endpoint:** `POST /resources`
- **Description:** Creates a new resource.
- **Request Headers:**
  - `Authorization: Bearer <token>`
- **Request Body:**
  ```json
  {
    "name": "New Resource Description"
  }
  ```
- **Success Response (201 Created):**
  ```json
  {
    "success": true,
    "data": {
      "id": "2",
      "name": "New Resource Description"
    }
  }
  ```

## Status Codes

We use semantic HTTP status codes:

| Code | Status | Description |
|---|---|---|
| 200 | OK | Request was successful. |
| 201 | Created | Resource successfully created. |
| 400 | Bad Request | Request payload was invalid or malformed. |
| 401 | Unauthorized | Authentication token is missing or invalid. |
| 403 | Forbidden | Authenticated user lacks permission to perform the action. |
| 404 | Not Found | The requested resource does not exist. |
| 422 | Unprocessable Entity | Semantically invalid data (e.g., failed validation rules). |
| 500 | Internal Server Error | An unexpected server error occurred. |