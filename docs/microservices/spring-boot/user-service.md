---
sidebar_position: 2
---

# User Service

Spring Boot service for user profile management, settings, and user-related operations.

## Overview

| Property | Value |
|----------|-------|
| **Port** | 8082 |
| **Database** | MySQL |
| **Framework** | Spring Boot 3.x |
| **Language** | Java 21 |

## Features

- User profile CRUD operations
- User search and discovery
- User settings management
- Blocking/unblocking users
- Contact synchronization
- FCM token management
- Device linking

## API Endpoints

### Users

```http
GET    /api/users
GET    /api/users/{id}
POST   /api/users
PUT    /api/users/{id}
DELETE /api/users/{id}
GET    /api/users/search?q={query}
GET    /api/users/me
```

### Profile

```http
GET  /api/users/{id}/profile
PUT  /api/users/{id}/profile
POST /api/users/{id}/avatar
```

### Settings

```http
GET  /api/users/{id}/settings
PUT  /api/users/{id}/settings
PUT  /api/users/{id}/settings/notifications
PUT  /api/users/{id}/settings/privacy
```

### Blocking

```http
GET    /api/users/{id}/blocked
POST   /api/users/{id}/block
DELETE /api/users/{id}/unblock/{blockedId}
```

### Contacts

```http
GET  /api/users/{id}/contacts
POST /api/users/{id}/contacts/sync
```

### Devices

```http
GET    /api/users/{id}/devices
POST   /api/users/{id}/devices
DELETE /api/users/{id}/devices/{deviceId}
POST   /api/users/{id}/fcm-token
```

## Data Models

### User

```java
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.UUID)
    private String id;

    @Column(unique = true)
    private String email;

    @Column(unique = true)
    private String username;

    private String displayName;
    private String avatarUrl;
    private String bio;
    private String phoneNumber;

    @Enumerated(EnumType.STRING)
    private UserRole role = UserRole.USER;

    @Enumerated(EnumType.STRING)
    private UserStatus status = UserStatus.ACTIVE;

    private boolean isBanned;
    private String banReason;
    private LocalDateTime bannedAt;

    private boolean isVerified;
    private LocalDateTime verifiedAt;

    private LocalDateTime lastActiveAt;

    @CreatedDate
    private LocalDateTime createdAt;

    @LastModifiedDate
    private LocalDateTime updatedAt;
}
```

### User Settings

```java
@Entity
@Table(name = "user_settings")
public class UserSettings {
    @Id
    private String userId;

    // Notification settings
    private boolean emailNotifications = true;
    private boolean pushNotifications = true;
    private boolean smsNotifications = false;

    // Privacy settings
    @Enumerated(EnumType.STRING)
    private PrivacyLevel profileVisibility = PrivacyLevel.PUBLIC;

    @Enumerated(EnumType.STRING)
    private PrivacyLevel lastSeenVisibility = PrivacyLevel.CONTACTS;

    private boolean readReceipts = true;
    private boolean typingIndicators = true;

    // Preferences
    private String language = "en";
    private String timezone = "UTC";
    private String theme = "system";

    @LastModifiedDate
    private LocalDateTime updatedAt;
}
```

## Configuration

```yaml
server:
  port: 8082

spring:
  datasource:
    url: jdbc:mysql://${MYSQL_HOST:localhost}:3306/quckchat_users
    username: ${MYSQL_USER:root}
    password: ${MYSQL_PASSWORD:password}

  servlet:
    multipart:
      max-file-size: 10MB
      max-request-size: 10MB

storage:
  type: s3
  bucket: quckchat-avatars
  region: ${AWS_REGION:us-east-1}
```

## Service Implementation

```java
@Service
@Transactional
public class UserService {

    public User createUser(CreateUserDto dto) {
        if (userRepository.existsByEmail(dto.getEmail())) {
            throw new UserAlreadyExistsException("Email already registered");
        }

        User user = User.builder()
            .email(dto.getEmail())
            .username(generateUsername(dto.getEmail()))
            .displayName(dto.getDisplayName())
            .build();

        user = userRepository.save(user);

        // Create default settings
        userSettingsRepository.save(UserSettings.builder()
            .userId(user.getId())
            .build());

        // Publish event
        kafkaTemplate.send("quckchat.users.events",
            new UserCreatedEvent(user.getId(), user.getEmail()));

        return user;
    }
}
```

## Kafka Events

```java
public class UserCreatedEvent {
    private String userId;
    private String email;
    private LocalDateTime timestamp;
}

public class UserUpdatedEvent {
    private String userId;
    private Map<String, Object> changes;
    private LocalDateTime timestamp;
}

public class UserBannedEvent {
    private String userId;
    private String reason;
    private String bannedBy;
    private LocalDateTime timestamp;
}
```
