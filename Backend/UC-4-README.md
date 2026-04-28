# 💬 InkWell Comment Service (UC-4)

![.NET 8](https://img.shields.io/badge/.NET-8.0-blueviolet.svg)
![MassTransit](https://img.shields.io/badge/MassTransit-Messaging-success.svg)

The **Comment Service** is a dedicated microservice for handling user interactions, discussions, and community engagement. 

## 🌟 Key Capabilities

- **Hierarchical Discussions**: Supports recursive data structures to enable top-level comments and infinite-depth nested replies.
- **Moderation Engine**: Implements Soft-Delete (`Status = 'Deleted'`) and Admin Approval/Rejection workflows to maintain community standards.
- **Engagement Metrics**: Tracks individual comment likes independent of post likes.
- **Asynchronous Alerts**: Emits the `CommentAddedEvent` to the Service Bus, which is intercepted by the Notification Service to alert the original author without impacting the API response time for the commenter.

## 🏗️ Technical Implementation

### Recursive Queries
Optimized Entity Framework queries to fetch parent-child comment trees efficiently.
```csharp
// Example model relationship
public Guid? ParentCommentId { get; set; }
```

## 📡 API Surface (Via Gateway)

| Endpoint | Method | Role | Description |
|----------|--------|------|-------------|
| `/api/comments/post/{postId}` | `GET` | Public | Retrieves all top-level comments |
| `/api/comments/{id}/replies` | `GET` | Public | Retrieves nested replies |
| `/api/comments` | `POST` | Authenticated | Submits a new comment/reply |
| `/api/comments/{id}` | `DELETE` | Owner/Admin | Soft deletes a comment |
