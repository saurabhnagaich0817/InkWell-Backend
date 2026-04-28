# ✍️ InkWell Post Service (UC-3)

![.NET 8](https://img.shields.io/badge/.NET-8.0-blueviolet.svg)
![EF Core](https://img.shields.io/badge/EF_Core-8.0-5C2D91.svg)
![MassTransit](https://img.shields.io/badge/MassTransit-Messaging-success.svg)

The **Post Service** is the core domain of the InkWell platform, managing the entire lifecycle of stories/articles. It leverages asynchronous communication to decouple content creation from notification delivery.

## 🌟 Key Capabilities

- **Rich Content Management**: Handles HTML/Quill content, cover image mappings, and automated SEO-friendly slug generation with collision detection.
- **State Machine**: Manages article states (Draft, Published, Archived).
- **Engagement Analytics**: Tracks high-frequency engagement metrics (Likes, Views, Saves) optimizing read performance.
- **Event Producer**: Emits `PostCreatedEvent` and `PostLikedEvent` via MassTransit. This decoupled design ensures the Post Service remains highly available and is not blocked by slow SMTP servers or heavy notification logic.

## 🏗️ Technical Implementation

### Database Design
The core entity `Post` maintains denormalized references (`AuthorName`, `CategoryName`) to optimize read queries without requiring synchronous inter-service HTTP calls to the Auth or Category services.

### Message Broker Integration
```csharp
// Example: Fire-and-forget event publishing
await _publishEndpoint.Publish(new PostCreatedEvent {
    PostId = post.PostId,
    AuthorId = post.AuthorId,
    Title = post.Title
});
```

## 📡 API Surface (Via Gateway)

| Endpoint | Method | Role | Description |
|----------|--------|------|-------------|
| `/api/posts` | `GET` | Public | Retrieves paginated published posts |
| `/api/posts/{slug}` | `GET` | Public | Retrieves specific post by URL slug |
| `/api/posts` | `POST` | Author/Admin| Creates a new story |
| `/api/posts/{id}/like`| `POST` | Authenticated | Toggles a like on a post |

## ⚙️ Environment Configuration

```json
{
  "RabbitMQ": {
    "Host": "localhost",
    "Username": "guest",
    "Password": "guest"
  }
}
```
