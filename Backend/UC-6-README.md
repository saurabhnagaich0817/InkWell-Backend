# 🖼️ InkWell Media Service (UC-6)

![.NET 8](https://img.shields.io/badge/.NET-8.0-blueviolet.svg)

The **Media Management Service** is an isolated component responsible for handling all I/O operations related to user-uploaded content, ensuring the core database remains lightweight.

## 🌟 Key Capabilities

- **Multipart Form Uploads**: Securely processes binary file uploads via `IFormFile`.
- **Static Asset Delivery**: Configures ASP.NET Core `UseStaticFiles` to serve content securely via HTTP boundaries (`/api/media/uploads/{filename}`).
- **MIME Type Validation**: Ensures only secure image formats (JPG, PNG, WEBP) are processed.
- **Accessibility (a11y)**: Enforces and manages `alt-text` metadata for all uploaded media.
- **Garbage Collection**: Exposes endpoints for safe deletion of unlinked or orphaned media files from the filesystem.

## 📡 API Surface (Via Gateway)

| Endpoint | Method | Role | Description |
|----------|--------|------|-------------|
| `/api/media/upload` | `POST` | Authenticated | Uploads an image and returns a permanent URL |
| `/api/media/{id}` | `DELETE` | Owner/Admin | Deletes a physical file and its metadata |
| `/api/media/{id}/alt-text` | `PATCH` | Owner | Updates accessibility text |
