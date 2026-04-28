# 📧 InkWell Newsletter Service (UC-7)

![.NET 8](https://img.shields.io/badge/.NET-8.0-blueviolet.svg)
![MassTransit](https://img.shields.io/badge/MassTransit-Messaging-success.svg)
![MailKit](https://img.shields.io/badge/MailKit-SMTP-D32F2F.svg)

The **Newsletter Service** is a specialized background worker and API combination designed to manage subscriber lists and broadcast high-volume emails without blocking interactive user requests.

## 🌟 Key Capabilities

- **Subscriber Management**: Exposes API endpoints to Subscribe, Unsubscribe, and manage subscription statuses.
- **Event-Triggered Broadcasting**: Intercepts `PostCreatedEvent` payloads via MassTransit.
- **Asynchronous Emailing**: Iterates over all active subscribers and dispatches beautifully formatted HTML emails containing dynamic content (Post Title, Links) via an SMTP relay (MailKit).
- **Admin Visibility**: Provides Admins with dashboards to monitor active subscriber counts.

## 🏗️ Technical Implementation

### Message Consumption
The service operates as a MassTransit `IConsumer<PostCreatedEvent>`, decoupling email delivery latency from the Post Service.
```csharp
public async Task Consume(ConsumeContext<PostCreatedEvent> context) {
    // Fetches all Active subscribers
    // Dispatches parallel background emails via ISmtpClient
}
```

## 📡 API Surface (Via Gateway)

| Endpoint | Method | Role | Description |
|----------|--------|------|-------------|
| `/api/newsletter/subscribe` | `POST` | Public | Adds a user email to the active broadcast list |
| `/api/newsletter/subscribers` | `GET` | Admin | Retrieves all subscribers for auditing |
