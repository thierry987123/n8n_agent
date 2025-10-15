# n8n – Clone and Run

Follow these steps to run this n8n setup locally using Docker Compose.

## Prerequisites
- Docker and Docker Compose installed

## Get the code
```bash
git clone https://github.com/thierry987123/n8n_agent.git
cd n8n_agent
```

## Start n8n
```bash
docker compose up -d
```

## Open n8n
- http://localhost:5678

## Data persistence
- Local data is stored in the `data/` folder (bind-mounted to `/home/node/.n8n`).
- To reset to a clean state, stop services and remove the `data/` folder, then start again.

## Sharing workflows
- Export from your n8n UI and place JSON files in `workflows/` to share.
- Your colleague can import those JSON files via the n8n UI.

## About this setup
- `docker-compose.yml` (version 3.7) defines:
  - Service `n8n` using `n8nio/n8n:latest`
  - Ports: `5678:5678`
  - Volumes:
    - `./data:/home/node/.n8n` (n8n app data)
    - `./uploads:/data` (optional local files you want to read from n8n)
  - Environment:
    - `GENERIC_TIMEZONE` (default `Asia/Ho_Chi_Minh`)
    - File access flags to allow reading host files under `/data` and n8n data paths
  - Healthcheck on `/healthz` and `restart: unless-stopped`

## Project folders
- `docker-compose.yml`: main compose file for n8n
- `data/`: persistent n8n data (database, credentials, settings, etc.)
- `uploads/`: put local files here to access them in n8n at `/data/...`
- `workflows/`: exported workflow JSON files to share
- `.gitignore`: excludes local data, uploads, and env files
- `git.sample`: sample commands to set up git username/email and remote
