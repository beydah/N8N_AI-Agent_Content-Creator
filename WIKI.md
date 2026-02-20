# 📖 Project Wiki: N8N AI Agent Ecosystem

Welcome to the official technical knowledge base for the **N8N AI Agent Ecosystem**. This wiki provides deep-dive information for developers and architects looking to build, scale, and manage specialized AI agents using n8n.

---

## 🏗️ Architecture Overview

Our ecosystem is built on a **Modular Monolith** pattern using n8n. Each agent is a standalone workflow but follows strict structural and communication standards to ensure interoperability.

### The "Core + Agent" Model
1. **Core (n8n Instance)**: Handles credentials, binary storage (Google Drive), and global triggers.
2. **Specialized Agents**: Discrete workflows located in `/agent/{name}` that perform specific reasoning or automation tasks.

---

## 🚀 Agent Development Lifecycle

To maintain high standards, every new agent must go through these phases:

### 1. Requirements Definition
- Identify the input source (e.g., Manual, Webhook, Schedule).
- Define the AI model requirements (Gemini 1.5 Pro vs Flash).
- List required third-party integrations (APIs).

### 2. Standardized Folder Structure
Every agent must reside in its own subdirectory:
```text
agent/
└── your-agent-name/
    ├── agent.json (Workflow Export)
    └── README.md  (Agent Documentation)
```

### 3. Workflow Design Standards
- **Node Naming**: Use descriptive, snake_case or Title Case names.
- **Error Handling**: Use "On Error -> Continue" with dedicated error-handling paths for production reliability.
- **Memory**: Implement persistent memory (Window Buffer or Database) for agents that require multi-turn context.

---

## 🔗 Inter-Agent Communication

Agents can communicate with each other using two primary methods:

### Method A: Execute Workflow Node (Synchronous)
Used when one agent needs a real-time result from another (e.g., a "Content Writer" calling a "SEO Analyzer").
- ✅ Fast, shares same n8n resources.
- ❌ Harder to scale horizontally.

### Method B: Webhooks / HTTP Request (Asynchronous)
Used for cross-instance or decoupled communication.
- ✅ Highly scalable, allows for external triggers.
- ❌ Slightly higher latency.

---

## 🛡️ Enterprise Deployment

For production environments, follow these hardening steps:

1. **Dockerization**: Always run n8n in a containerized environment.
2. **Reverse Proxy**: Use Nginx or Traefik with SSL (Let's Encrypt).
3. **Database**: Use PostgreSQL for persistent n8n data instead of the default SQLite.
4. **Credential Isolation**: Use environment variables for sensitive API keys.

Refer to [SECURITY.md](https://github.com/beydah/N8N_AI-Agent/blob/main/SECURITY.md) for more details.

---

## ❓ Frequently Asked Questions

### How do I import a new agent?
Download the `agent.json` from the repository, go to n8n, click "Import from File," and configure your credentials.

### Which AI models are supported?
While this repo defaults to **Google Gemini**, the framework is designed to work with OpenAI, Anthropic, or local models (via Ollama) by swapping the AI nodes.

### Can I run this on a local machine?
Yes, using Docker Desktop. However, webhooks from social media (LinkedIn) or WordPress may require a tunnel (e.g., Ngrok) or a public IP.

---
[🔙 Back to Main README](https://github.com/beydah/N8N_AI-Agent)
