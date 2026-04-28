# 🔐 InkWell Auth Service (UC-2)

![.NET 8](https://img.shields.io/badge/.NET-8.0-blueviolet.svg)
![EF Core](https://img.shields.io/badge/EF_Core-8.0-5C2D91.svg)
![JWT](https://img.shields.io/badge/JWT-Security-black.svg)
![RabbitMQ](https://img.shields.io/badge/RabbitMQ-Event_Driven-FF6600.svg)

The **Authentication & Identity Service** is the guardian of the InkWell platform. It handles user registration, secure authentication, role-based access control (RBAC), and profile management.

## 🌟 Key Capabilities

- **Robust Authentication**: Supports standard Email/Password authentication using BCrypt hashing algorithms.
- **SSO Integration**: Full Google OAuth 2.0 integration for frictionless user onboarding.
- **Stateless Authorization**: Issues hardened JWTs (JSON Web Tokens) with defined scopes and role claims (`Reader`, `Author`, `Admin`).
- **Event-Driven Synchronization**: Broadcaster of core identity events via **MassTransit/RabbitMQ** (`UserRegisteredEvent`, `UserLoggedInEvent`), ensuring downstream services (like Notification and Post services) have synchronized, read-optimized user projections without synchronous HTTP coupling.

## 🏗️ Technical Implementation

### Core Libraries
- `Microsoft.AspNetCore.Authentication.JwtBearer`
- `BCrypt.Net-Next`
- `Google.Apis.Auth`
- `MassTransit.RabbitMQ`

### Database Schema Context
Manages the `Users` table within `InkWellDB`, isolating sensitive PII and password hashes from other domain boundaries.

## 📡 API Surface (Via Gateway)

| Endpoint | Method | Role | Description |
|----------|--------|------|-------------|
| `/api/auth/register` | `POST` | Public | Registers a new user account |
| `/api/auth/login` | `POST` | Public | Authenticates and returns JWT |
| `/api/auth/google-login` | `POST` | Public | Verifies Google ID Token |
| `/api/auth/profile` | `GET` | Authenticated | Retrieves current user profile |

## ⚙️ Environment Configuration

Ensure the following variables are present in `appsettings.json` or Environment Variables:
```json
{
  "Jwt": {
    "Key": "YOUR_SUPER_SECRET_KEY_MIN_32_CHARS",
    "Issuer": "InkWellAuthService",
    "Audience": "InkWellClients"
  },
  "Google": {
    "ClientId": "YOUR_GOOGLE_CLIENT_ID"
  }
}
```
