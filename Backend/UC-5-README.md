# 🏷️ InkWell Category Service (UC-5)

![.NET 8](https://img.shields.io/badge/.NET-8.0-blueviolet.svg)

The **Category & Taxonomy Service** provides structural metadata for the InkWell platform, allowing posts to be classified and discovered efficiently.

## 🌟 Key Capabilities

- **Taxonomy Management**: CRUD operations for broad categories (e.g., *Technology*, *Lifestyle*).
- **Tag Management**: CRUD operations for granular, many-to-many tags (e.g., `#dotnet`, `#microservices`).
- **Automated Slugification**: Converts category names into URL-safe slugs (`Machine Learning` -> `machine-learning`) for SEO-friendly routing.
- **Reference Integrity**: Ensures that posts mapped to categories maintain referential validity.

## 📡 API Surface (Via Gateway)

| Endpoint | Method | Role | Description |
|----------|--------|------|-------------|
| `/api/categories` | `GET` | Public | Retrieves all active categories |
| `/api/categories` | `POST` | Admin | Creates a new category |
| `/api/tags` | `GET` | Public | Retrieves all tags |
