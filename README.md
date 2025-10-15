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
