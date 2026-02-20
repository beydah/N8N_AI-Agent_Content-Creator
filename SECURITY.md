# Security Policy

## 🔒 Security Overview

The **N8N AI Agent Content Creator** is committed to ensuring the security of its users and their data. This policy outlines how we handle security vulnerabilities and provides best practices for secure deployment.

## 🛡️ Supported Versions

We actively provide security updates for the following versions:

| Version | Supported |
| ------- | --------- |
| Latest  | ✅ Yes     |
| < 1.0   | ❌ No      |

## 🚨 Reporting a Vulnerability

**Please do not open public issues for security vulnerabilities.**

If you discover a security-related issue, please report it through one of the following channels:

1. **GitHub Security Advisory**: Use the [Private reporting](https://github.com/beydah/N8N_AI-Agent_Content-Creator/security/advisories/new) feature.
2. **Email**: Send detailed information to `security@beydah.dev` (replace with actual site if different).

### What to include in your report:
- A description of the vulnerability and its potential impact.
- Steps to reproduce the issue (PoC).
- Any suggested fixes or mitigations.

We aim to acknowledge all reports within **24 hours** and provide a resolution timeline based on severity.

## 🔐 Security Best Practices

### 1. Secret Management
- **Never hardcode API keys** in your n8n workflows.
- Use **Environment Variables** or n8n's built-in **Credentials** system.
- Ensure your `.env` file is never committed to version control.

### 2. Deployment Security
- If deploying via Docker, run containers as **non-root users**.
- Use **HTTPS** for all n8n traffic (via Nginx/Reverse Proxy).
- Regularly update your Docker images to the latest stable versions.

### 3. Network Access
- Restrict access to your n8n instance using **Basic Auth** or **OAuth2**.
- Only expose necessary ports (e.g., 443) to the public internet.

---
*Last Updated: 2026-02-20*
