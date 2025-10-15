## n8n local setup (Docker Compose)

This folder provides a minimal local setup for running n8n using Docker Compose with a bind-mounted data directory for persistence.

### Files
- `docker-compose.yml`: n8n service with port mapping, envs, healthcheck, and data bind mount
- `data/`: local directory created on first run to persist n8n data (credentials, workflows, etc.)
- `workflows/`: place exported workflow JSON files to share with others

### Quick start
1. Start services:
   ```bash
   docker compose -f docker-compose.yml up -d
   ```
2. Open n8n: `http://localhost:5678`

### Export and import workflows
- Export from your n8n:
  - In the n8n UI, open a workflow → three-dots menu → Export → "Download to file"
  - Save the JSON into `workflows/` and commit it to share (optional)
- Import on another machine:
  - In n8n UI → "Import from file" → select the workflow JSON

### Build a Docker image that includes workflows
- Goal: bake your workflows into an image so others can run n8n with flows preloaded.
- Steps:
  1. Export your workflows to JSON files and place them in `workflows/`
  2. Create a Dockerfile (example uses the official image and copies flows):
     ```Dockerfile
     FROM n8nio/n8n:latest
     # Copy pre-exported workflows into the container's data directory
     COPY workflows/ /home/node/.n8n/workflows/
     # Expose default port (documented; base image already exposes internally)
     EXPOSE 5678
     ```
  3. Build and tag your image:
     ```bash
     docker build -t your-dockerhub-username/n8n-with-flows:latest .
     ```
  4. Run the image:
     ```bash
     docker run -d --name n8n \
       -p 5678:5678 \
       your-dockerhub-username/n8n-with-flows:latest
     ```
  5. Push for sharing:
     ```bash
     docker login
     docker push your-dockerhub-username/n8n-with-flows:latest
     ```

### Data persistence
- Your local data is stored under `./data` on the host and mounted to `/home/node/.n8n` in the container.
- To reset n8n to a clean state, stop the stack and remove the `data/` folder.

### Useful commands
- Stop services:
  ```bash
  docker compose down
  ```
- View logs:
  ```bash
  docker compose logs -f n8n
  ```
- Recreate after compose changes:
  ```bash
  docker compose up -d --force-recreate
  ```
