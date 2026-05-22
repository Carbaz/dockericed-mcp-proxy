# Dockerized MCP Proxy

Expose a STDIO-based MCP server as an MCP SSE endpoint using
[`mcp-proxy`](https://github.com/sparfenyuk/mcp-proxy), packaged in a Docker container.

The core idea: instead of each client spawning its own STDIO subprocess,
a single `mcp-proxy` instance wraps the MCP server and exposes it over HTTP/SSE,
making it accessible to multiple clients simultaneously, including remote ones.

```text
Client  ◄──Sse──►  mcp-proxy (:8000/sse)  ◄──stdio──►  mcp-server-fetch
```

```mermaid
flowchart LR
    A[Client] <-->|Sse| B[mcp-proxy\n:8000/sse]
    B <-->|stdio| C[mcp-server-fetch]
```

## Contents

| File                                               | Purpose                                                                             |
| -------------------------------------------------- | ----------------------------------------------------------------------------------- |
| [`Dockerfile`](Dockerfile)                         | Practical example: Minimal image that runs `mcp-proxy` wrapping `mcp-server-fetch`. |
| [`docker-compose.yml`](docker-compose.yml)         | Builds and launches the Docker service.                                             |
| [`proxy_fetch_demo.ipynb`](proxy_fetch_demo.ipynb) | Notebook walks through the concept, local setup and Docker workflow step by step.   |
|                                                    |                                                                                     |

## Quick Start

**Requirements:** Docker

```bash
# Build and start the service
docker compose up --build -d

# The SSE endpoint is now available at:
# http://localhost:8000/sse

# Stop the service
docker compose down
```

Connect any MCP SSE client to `http://localhost:8000/sse`.\
To change the host port, edit the `ports` mapping in `docker-compose.yml`.
