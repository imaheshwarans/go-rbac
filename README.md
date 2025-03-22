# go-rbac

## Overview
The go-rbac microservice is responsible for managing users, roles, and permissions within an application. It validates roles and permissions using Bearer tokens and provides secure access control through CRUD operations.

## Tech Stack
- **Language:** Go
- **Framework:** Gorilla Mux (for REST API)
- **Database:** PostgreSQL
- **Authentication:** JWT (JSON Web Token)
- **Authorization:** Middleware-based role and permission checks
- **Storage:** Redis (for caching session data)
- **Deployment:** Docker, Kubernetes

## Features
- Secure JWT-based authentication and authorization.
- Fine-grained role-based access control (RBAC).
- Role and permission management.
- User management with role assignments.
- Middleware for role validation.
- Secure API endpoints with rate limiting.
- Scalable microservice architecture.

## Components
### 1. **Authentication Service**
- Issues JWT Bearer tokens upon successful login.
- Validates tokens received in API requests.
- Ensures the token contains necessary role-based claims.
- Supports token revocation and refresh mechanisms.

### 2. **RBAC Middleware**
- Extracts the token from the request header.
- Validates the token using a JWT verification process.
- Extracts user roles from the token claims.
- Checks if the user role has the necessary permissions for the requested operation.
- Logs authentication failures and unauthorized access attempts.
- Returns a 403 Forbidden response if access is denied.

### 3. **Role and Permission Management**
- Provides CRUD operations for managing roles and permissions.
- Maps permissions to roles dynamically to allow flexible access control.
- Enforces hierarchical role relationships (e.g., Admin > Manager > User).
- Supports the ability to assign multiple roles to a single user.

### 4. **User Management**
- Supports user registration and authentication.
- Allows assignment and removal of roles to users.
- Maintains a user-role mapping table for efficient access control.
- Provides CRUD operations for managing user details and statuses (active/inactive).

### 5. **Authorization Logic**
- Maps each API request to a predefined permission.
- Ensures that the user’s assigned roles contain the necessary permissions.
- Implements fine-grained access control by restricting operations at the resource level.
- Allows super admin users to manage roles, users, and permissions without restriction.

## Database Schema
- Users table
- Roles table
- Permissions table
- Role-Permissions mapping table

## API Endpoints
- **Authentication**: Login, token validation
- **User management**: Create, update, delete, list users
- **Role management**: Create, update, delete, list roles
- **Permission management**: Create, update, delete, list permissions

## Folder Structure
```
go-rbac/
│── cmd/               # Main application entrypoint
│── internal/          # Business logic and internal implementation
│   ├── auth/          # JWT authentication handlers
│   ├── handlers/      # HTTP request handlers
│   ├── middleware/    # Middleware for authentication & authorization
│   ├── models/        # Database models & schemas
│   ├── repository/    # Data persistence logic
│   ├── services/      # Business logic for roles, users, permissions
│── config/            # Configuration files
│── migrations/        # Database migration scripts
│── main.go            # Application entrypoint
│── go.mod             # Module dependencies
```

## Security and Performance Enhancements
### 1. **Logging & Error Handling**
- Implement structured logging using Logrus or Zap for better debugging.
- Capture and log all authentication failures, permission denials, and API errors.
- Return meaningful HTTP status codes and error messages.

### 2. **Rate Limiting & Security Measures**
- Implement rate limiting using middleware to prevent abuse (e.g., Redis-based token bucket).
- Secure API endpoints against SQL injection, XSS, and CSRF attacks.
- Use HTTPS for secure communication and enforce strong JWT secrets.

### 3. **Caching Strategy**
- Store frequently accessed data like role-permission mappings in Redis.
- Implement cache invalidation policies to sync with database updates.
- Optimize database queries to minimize redundant lookups.

### 4. **Unit & Integration Testing**
- Write unit tests for authentication, authorization, and business logic using Go’s testing package.
- Use mock databases for integration testing.
- Validate API responses using Postman or automated test frameworks.

## Deployment
- **Containerization**: Use Docker for packaging.
- **Orchestration**: Deploy via Kubernetes.
- **API Gateway**: Use Kong or Nginx for managing API requests.
- **Monitoring**: Use Prometheus and Grafana for monitoring service health.

## Notes
This RBAC microservice ensures secure role-based access control in a scalable manner. By integrating JWT-based authentication, role-based permission checks, and a structured database model, it provides an efficient and secure way to manage user access in microservices architectures.

