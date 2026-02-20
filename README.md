# 🤖 N8N AI Agents

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![n8n](https://img.shields.io/badge/n8n-Workflow-orange.svg)](https://n8n.io)
[![Gemini AI](https://img.shields.io/badge/AI-Gemini-blue.svg)](https://ai.google.dev/)

A modular, enterprise-ready automated agent ecosystem built on n8n. This framework provides a scalable architecture for deploying specialized AI agents to handle complex workflows, content generation, and cross-platform automation, designed to evolve with your business needs.

---

## 🚀 Key Features

- **Modular Agent Framework**: Scalable architecture to deploy specialized agents for content creation, lead generation, and data extraction.
- **Cross-Platform Orchestration**: Seamlessly connect AI reasoning with WordPress, LinkedIn, CRMs, and custom APIs.
- **Intelligent State Management**: Persistent memory across workflows to ensure context-aware and non-repetitive actions.
- **Enterprise-Ready Automation**: Robust, production-grade n8n workflows designed for reliability and scalability.
- **Multi-Model Flexibility**: Built to leverage advanced LLMs like Gemini for high-reasoning and creative tasks.

---

## 🤖 Available Agents

| Agent Name         | Folder                                                       | Description                                                 | Status  |
| :----------------- | :----------------------------------------------------------- | :---------------------------------------------------------- | :------ |
| **Content Writer** | [`agent/contect_creator`](./agent/contect_creator/README.md) | Generates blog posts, images, and publishes to WP/LinkedIn. | ✅ Ready |

> [!NOTE]
> All agents are designed to be interoperable. Check individual READMEs for specific API requirements and workflow diagrams.

---

## 🔧 Core Requirements

- **n8n** (Docker-based production setup recommended)
- **Google Gemini API Key**

---

## 📦 Installation & Setup

### 1. Environment Setup
We recommend using Docker for a robust n8n environment. See [SECURITY.md](./SECURITY.md) for deployment best practices.

### 2. Credential Configuration
For each agent, you will need to configure:
- **Google Gemini**: [AI Studio](https://aistudio.google.com/app/apikey)

### 3. Importing Agents
Navigate to your n8n instance and:
1. Create a new workflow.
2. Import the `.json` file from the specific agent directory.
3. Configure credentials and test.

---

## 📚 Documentation & Wiki

For in-depth technical details, architecture diagrams, and enterprise deployment guides, please visit our **[Project Wiki](https://github.com/beydah/N8N_AI-Agent/wiki)**.

---

## 🤝 Contributing

We welcome contributions! Please review our [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines on:
- Code style and naming conventions.
- **Agent contribution rules** (directory structure and interoperability).
- Pull request process.

---

## 🛡️ Security

Security is a top priority. Please refer to [SECURITY.md](./SECURITY.md) for vulnerability reporting and secure configuration tips.

## 📄 License

This repo is licensed under the **MIT License** - see the [LICENSE](./LICENSE) file for details.

---
*Created with ❤️ by [Ilkay Beydah Saglam](https://github.com/beydah)*
