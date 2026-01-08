---
sidebar_position: 3
---

# Notification Service

NestJS service for push notifications, email, and SMS delivery.

## Overview

| Property | Value |
|----------|-------|
| **Port** | 3001 |
| **Database** | PostgreSQL |
| **Framework** | NestJS 10.x |
| **Language** | TypeScript |

## Features

- Push notifications (FCM, APNs)
- Email notifications (SendGrid, SES)
- SMS notifications (Twilio)
- Notification preferences
- Delivery tracking
- Template management

## Notification Channels

```typescript
enum NotificationChannel {
  PUSH = 'push',
  EMAIL = 'email',
  SMS = 'sms',
  IN_APP = 'in_app',
}

interface NotificationPayload {
  userId: string;
  type: NotificationType;
  title: string;
  body: string;
  data?: Record<string, any>;
  channels: NotificationChannel[];
}
```

## Kafka Consumer

```typescript
@EventPattern('quckchat.notifications.events')
async handleNotification(payload: NotificationPayload) {
  const userPrefs = await this.getUserPreferences(payload.userId);

  for (const channel of payload.channels) {
    if (this.isChannelEnabled(userPrefs, channel)) {
      await this.sendToChannel(channel, payload);
    }
  }
}
```
