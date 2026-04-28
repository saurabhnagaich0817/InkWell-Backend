# 🔔 InkWell Notification Service (UC-8)

![.NET 8](https://img.shields.io/badge/.NET-8.0-blueviolet.svg)
![MassTransit](https://img.shields.io/badge/MassTransit-Messaging-success.svg)
![SignalR Ready](https://img.shields.io/badge/Architecture-Event_Driven-10b981.svg)

The **Notification Service** is the nerve center of the InkWell platform's user engagement strategy. It acts as an asynchronous sink for platform-wide events, processing them into tangible alerts (in-app notifications and targeted emails).

## 🌟 Key Capabilities

- **Omni-Channel Alerts**: Generates persistent Database Notifications (for UI bells) and dispatches SMTP emails (for offline users) simultaneously.
- **Event Aggregation**: A single service that listens to:
  - `UserRegisteredEvent` (Sends Welcome Emails, Alerts Admins)
  - `PostCreatedEvent` (Broadcasts 'New Story' alerts to the follower network)
  - `CommentAddedEvent` (Alerts Authors of new interactions)
  - `PostLikedEvent` (Alerts Authors of engagement)
- **Local Read Projections**: Maintains a read-optimized, synced copy of the User table (`NotificationUsers`) to process notifications at high speed without executing synchronous HTTP queries to the Auth Service.
- **Polling / Real-Time Ready**: Exposes `/api/notifications` endpoints consumed by the Angular frontend via 30s long-polling mechanisms.

## 🏗️ Technical Implementation

### Core Dependencies
- `MassTransit.RabbitMQ` for durable, guaranteed message delivery.
- `MailKit` for background SMTP dispatching.
- `Entity Framework Core` for fast, indexed querying of unread notification states.

## 📡 API Surface (Via Gateway)

| Endpoint | Method | Role | Description |
|----------|--------|------|-------------|
| `/api/notifications` | `GET` | Authenticated | Fetches unread & recent notifications |
| `/api/notifications/{id}/read`| `PATCH` | Owner | Marks a specific notification as read |
| `/api/notifications/read-all`| `PATCH` | Owner | Acknowledges all pending alerts |
