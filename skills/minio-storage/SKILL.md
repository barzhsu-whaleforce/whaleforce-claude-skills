---
name: minio-storage
description: S3-compatible object storage service for 13F holding data. Use when user needs to download 13F filings, institutional investor holdings, or access MinIO/S3 storage.
---

# MinIO Storage Service

Owner: Yuhsiang

## Connection Info

| Item | Value |
|------|-------|
| **Web UI** | `https://minio.gpu5090.whaleforce.dev` |
| **API (Internal)** | `https://minio.api.gpu5090.whaleforce.dev` |
| **API (External)** | `https://minio.api.whaleforce.dev` |
| **Health Check** | `/minio/health/live` |
| **Account** | `whaleforce` |
| **Password** | `whaleforce.ai` |
| **Default Bucket** | `13f` |

## Quick Start - mc CLI (Recommended)

```bash
# Setup alias (once)
mc alias set wf https://minio.api.gpu5090.whaleforce.dev whaleforce whaleforce.ai

# Download entire 13f bucket
mc mirror wf/13f ./local_13f/

# Download specific year
mc mirror wf/13f/2024/ ./local_13f/2024/

# List files
mc ls wf/13f/
```

## Quick Start - Python boto3

```python
import boto3
from botocore.client import Config

s3 = boto3.client(
    "s3",
    endpoint_url="https://minio.api.gpu5090.whaleforce.dev",
    aws_access_key_id="whaleforce",
    aws_secret_access_key="whaleforce.ai",
    config=Config(signature_version="s3v4"),
)

# List files
response = s3.list_objects_v2(Bucket="13f", Prefix="2024/")
for obj in response.get("Contents", [])[:10]:
    print(f"{obj['Key']} - {obj['Size']} bytes")

# Download file
s3.download_file(Bucket="13f", Key="path/to/file.csv", Filename="local.csv")
```

## Data Coverage

| Item | Range |
|------|-------|
| **Years** | 2020 ~ 2025 |
| **Companies** | S&P 500 constituents |
| **Total Size** | ~23 GB |
| **File Count** | ~20,000 |

## Endpoint Selection

| Location | Endpoint | Speed |
|----------|----------|-------|
| **gpu5090 Internal** | `minio.api.gpu5090.whaleforce.dev` | **1.7 GB/s** |
| External | `minio.api.whaleforce.dev` | ~5 MB/s |

**Note**: On gpu5090, use internal endpoint for 1.7 GB/s speed - 23 GB in just 13 seconds!
