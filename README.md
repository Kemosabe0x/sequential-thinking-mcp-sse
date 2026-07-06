# Sequential Thinking MCP Server

This repository contains a Model Context Protocol (MCP) server that provides a `sequentialthinking` tool. The tool facilitates dynamic and reflective problem-solving through a chain of thoughts, allowing AI agents to break down complex problems, revise past thoughts, and explore branches of logic before arriving at a conclusion.

This project is configured to run in two modes:
1. **Stdio**: Standard MCP transport for local IDE and CLI integrations.
2. **Server-Sent Events (SSE)**: Deployed as a secure, remote HTTP gateway on Google Cloud Run.

## Local Development

Install dependencies and build the TypeScript code:
```bash
npm install
npm run build
```

To run the Stdio server locally:
```bash
node dist/index.js
```

## Cloud Run Deployment (SSE Gateway)

This project can be easily cloned and deployed to **any** Google Cloud account or project. This allows anyone to host their own secure, remote Sequential Thinking MCP server.

### Prerequisites

1. Install the [Google Cloud CLI (`gcloud`)](https://cloud.google.com/sdk/docs/install).
2. Authenticate with your Google account:
   ```bash
   gcloud auth login
   ```
3. Set your active Google Cloud Project (ensure billing is enabled):
   ```bash
   gcloud config set project YOUR_PROJECT_ID
   ```
4. Enable the required APIs for your project:
   ```bash
   gcloud services enable run.googleapis.com cloudbuild.googleapis.com
   ```

### Deploying

Once authenticated, you can deploy the service using the provided `Makefile`:

```bash
make deploy
```

**What this does:**
- Packages the application and builds a Docker container using the provided `Dockerfile`.
- Generates a secure, random `MCP_API_KEY` (if one is not already provided).
- Deploys the container to Google Cloud Run (default region: `us-central1`).
- Outputs the Service URL and your secure API key.

### Client Configuration

Once deployed, you can configure your MCP client (Cursor, Hermes, Antigravity, Claude Desktop, etc.) to connect to the SSE gateway. 

Provide your client with:
- **URL**: `<YOUR_CLOUD_RUN_URL>/sse`
- **Headers**: `Authorization: Bearer <YOUR_MCP_API_KEY>`
