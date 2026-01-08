---
sidebar_position: 5
---

# ML Service

Python service for machine learning predictions using Azure Databricks.

## Overview

| Property | Value |
|----------|-------|
| **Port** | 5008 |
| **Database** | Azure Databricks |
| **Framework** | FastAPI |
| **Language** | Python 3.11 |

## Features

- User recommendations
- Content ranking
- Spam detection
- Model serving

## Databricks Integration

```python
from databricks import sql

connection = sql.connect(
    server_hostname=os.environ["DATABRICKS_HOST"],
    http_path=os.environ["DATABRICKS_HTTP_PATH"],
    access_token=os.environ["DATABRICKS_TOKEN"]
)
```
