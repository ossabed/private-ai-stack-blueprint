# Neo Horizons · Private AI Stack Blueprint

**Own the memory. Rent the intelligence.**

This repository contains the source code and architecture for a sovereign, private AI workspace. It is designed for professionals who require data privacy, exit-readiness, and multi-device synchronization without relying on public cloud silos.

## 🚀 Live Interactive Blueprint
View the fully interactive, animated version here:  
👉 **[Neo Horizons Live Blueprint](https://oss.github.io/private-ai-stack-blueprint/)**

*(Replace the link above with your actual GitHub Pages URL)*

## 🏗️ Architecture Overview
The stack is built on three core principles:
1. **Local-First Memory:** Context lives in plain Markdown files indexed by a local MCP server.
2. **Zero Public Ports:** All access is secured via a Tailscale mesh network.
3. **Exit-Ready Design:** Every component is replaceable; no vendor lock-in.

### The Mermaid Source Code
You can regenerate the architecture diagram using any Mermaid-compatible tool or MCP server.

```mermaid
flowchart TB
  subgraph DEVICES["Any device · anywhere"]
    WIN["Windows desktop<br/>Qwen Desktop + MCP · Browser/PWA"]
    PHONE["Android / any phone<br/>Open WebUI PWA · Tailscale"]
    WA["WhatsApp<br/>E2E channel"]
  end
  subgraph TAILNET["Tailscale mesh · WireGuard · no public ports"]
    SERVE["Tailscale Serve<br/>HTTPS on *.ts.net"]
  end
  subgraph LAPTOP["Laptop · always on · Windows host"]
    subgraph VM["Hyper-V → Ubuntu VM · Docker"]
      OWUI["Open WebUI :8080<br/>Projects · Spaces · chat history"]
      OCLAW["OpenClaw :18789<br/>Agents · channels · OpenAI-compat API"]
      OMEM["OpenMemory :8765<br/>MCP memory · UI :3000"]
      FILES["Canonical files<br/>Markdown + git · Syncthing"]
    end
  end
  subgraph CLOUD["External · pay as you go"]
    OROUTER["OpenRouter<br/>no-training routing · ZDR · logging OFF"]
    MODELS["Qwen · Claude · 600+ models"]
    QPERSONAL["Qwen free app cloud<br/>personal lane · own memory"]
  end
  NAS["UGREEN NAS DH2300<br/>nightly encrypted backup · restore drills"]

  WIN --> SERVE
  PHONE --> SERVE
  SERVE --> OWUI
  SERVE --> OCLAW
  SERVE --> OMEM
  WA <--> OCLAW
  OWUI -->|inference| OROUTER
  OCLAW -->|inference| OROUTER
  OROUTER --> MODELS
  OWUI -->|MCP| OMEM
  OCLAW -->|MCP| OMEM
  WIN -->|local MCP| OMEM
  WIN -.->|personal chat| QPERSONAL
  FILES -->|nightly tar + rsync| NAS
  FILES -.->|Syncthing| NAS
