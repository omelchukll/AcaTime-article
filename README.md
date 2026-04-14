# AcaTime — Docker Quick Start

## What is included

- Anonymized real-world dataset (KNU-based)
- ITC 2019 dataset import example
- Precomputed schedule examples
- Full application (API + UI)
- PostgreSQL database snapshot

## Prerequisites

- Docker with Compose plugin installed

## Start

```bash
docker compose up -d --build
```

The database dump (~127 MB) is imported on first start — this takes **2–5 minutes**.

Open: **http://localhost:5000**

## Test Accounts

| Role | Login | Password |
|------|-------|----------|
| Faculty user (anonymized real dataset) | `test` | `qwerty` |
| ITC 2019 season import & statistics | `itc2019` | `itc2019` |

## Stop

```bash
# Stop (data preserved)
docker compose down

# Stop and wipe all data
docker compose down -v
```
