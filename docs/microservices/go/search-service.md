---
sidebar_position: 3
---

# Search Service

Go service for full-text search using Elasticsearch.

## Overview

| Property | Value |
|----------|-------|
| **Port** | 5006 |
| **Database** | MySQL + Elasticsearch |
| **Framework** | Gin |
| **Language** | Go 1.21 |

## API Endpoints

```http
GET /api/search?q={query}&type={type}
GET /api/search/messages?q={query}
GET /api/search/users?q={query}
GET /api/search/channels?q={query}
POST /api/search/reindex
```
