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

## Included workflow: `data/my workflow.json`
- This repo contains a workflow JSON located at `data/my workflow.json`.
- It demonstrates reading a local metadata file and using Gemini-based nodes, then writing results to Google Sheets.

### Import this workflow
1. Open n8n at `http://localhost:5678`
2. In the top-right menu, choose "Import from file"
3. Select `data/my workflow.json`
4. Review nodes and credentials (you may need to set up Google and Gemini credentials in n8n)
5. Save and run

### Update → Commit → Share → Re-import
- If you change the workflow in your local n8n, those changes are stored in `data/` (not tracked by git).
- To share updates via git:
  1. Export the updated workflow from n8n UI → "Download to file"
  2. Replace the JSON file in this repo (e.g., overwrite `data/my workflow.json`) or place it under `workflows/`
  3. Commit and push:
     ```bash
     git add data/my\ workflow.json  # or workflows/<file>.json
     git commit -m "chore: update workflow"
     git push
     ```
  4. Colleagues pull the latest changes and repeat the "Import from file" steps to load the updated workflow in their n8n
