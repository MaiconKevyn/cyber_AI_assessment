# QA Dashboard - Setup Guide

Dashboard for managing energy assets, devices, and vulnerabilities with AI chat assistant.

## Prerequisites

- Docker and Docker Compose
- OpenAI API Key

## Quick Start

### 1. Configure OpenAI API Key

```bash
cp .env.example .env
# Edit .env and add your API key:
# OPENAI_API_KEY=sk-...
```

### 2. Build and start services

```bash
sudo docker-compose build --no-cache
sudo docker-compose up -d
```

Wait 10 seconds for database initialization.

### 3. Start dashboard dev server

```bash
sudo docker exec -it qa-dashboard-app /start.sh
```

Keep this terminal open. The dashboard runs on this process.

### 4. Index documents (in a new terminal)

```bash
sudo docker exec -it qa-dashboard-chat-ai python app/scripts/index_documents.py
```

Expected output: `Total chunks indexed: 48`

### 5. Access the application

Open browser: **http://localhost:55480**

**Login credentials:**
- Admin: `admin-user@uorak.com` / `Password123!`
- Manager: `manager@uorak.com` / `Password123!`
- Operator: `operator-user@uorak.com` / `Password123!`

The AI chat widget appears in the bottom-right corner.

---

## Helper Scripts

```bash
# Rebuild everything from scratch
sudo ./rebuild-all.sh

# Rebuild only Chat AI service (after code changes)
sudo ./rebuild-chat.sh

# Start application (already built)
sudo ./start-all.sh
```

---

## Troubleshooting

**Chat shows "Failed to connect":**
```bash
sudo docker-compose restart chat-ai
sudo docker-compose logs chat-ai
```

**Dashboard not loading:**
Make sure you ran step 3: `sudo docker exec -it qa-dashboard-app /start.sh`

**Check container status:**
```bash
sudo docker-compose ps
```

All three containers (app, chat-ai, db) should show "Up" status.

---

## Architecture

- **Dashboard**: http://localhost:55480 (Next.js)
- **API**: http://localhost:55481
- **Chat AI**: http://localhost:5482 (FastAPI)
- **Database**: PostgreSQL with pgvector

For detailed documentation, see `chat-ai/README.md`.
