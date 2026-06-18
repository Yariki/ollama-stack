# Ollama + Open WebUI (Docker Compose)

This project runs a local LLM stack with two services:

- Ollama for model serving
- Open WebUI for a browser-based chat interface

Both services are orchestrated with Docker Compose and connected through an internal bridge network.

## What this setup provides

- Persistent Ollama model storage on your host
- Persistent Open WebUI data (database, settings, uploads)
- Configurable WebUI port
- Shared internal service network between Open WebUI and Ollama

## Prerequisites

- Docker Engine installed
- Docker Compose v2 available (`docker compose`)

## Configuration

The stack uses environment variables from `.env`.

Required variables:

- `OLLAMA_DATA_PATH`: host path for Ollama data directory
- `OPEN_WEBUI_DATA_PATH`: host path for Open WebUI backend data
- `WEBUI_SECRET_KEY`: stable secret for Open WebUI sessions/tokens

Optional variables:

- `OPEN_WEBUI_PORT`: host port for Open WebUI (default: `3000`)

Example values:

- `OLLAMA_DATA_PATH=/absolute/path/to/ollama-data`
- `OPEN_WEBUI_DATA_PATH=/absolute/path/to/open-webui-data`
- `WEBUI_SECRET_KEY=replace-with-a-long-random-secret`
- `OPEN_WEBUI_PORT=3000`

## Run

Start in detached mode:
```bash
  docker compose up -d
```

Stop services:
```bash
  docker compose stop
```

Stop and remove containers:
```bash
  docker compose down
```
View logs:
```bash
  docker compose logs -f
```
Create a secret key for Open WebUI:
```bash
  openssl rand -hex 32
```

## Access

- Open WebUI: `http://localhost:3000` (or the port from `OPEN_WEBUI_PORT`)
- Ollama API: `http://localhost:11434`

## Notes

- Keep `WEBUI_SECRET_KEY` stable across restarts/redeployments.
- Ensure the host directories in `OLLAMA_DATA_PATH` and `OPEN_WEBUI_DATA_PATH` exist and are writable.
- Open WebUI reaches Ollama through the Docker network using `http://ollama:11434`.