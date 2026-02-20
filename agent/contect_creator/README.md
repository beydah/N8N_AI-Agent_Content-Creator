# Agent - Content Writer ✍️

Automated SEO-friendly blog post and image generation agent powered by Google Gemini AI.

## 📄 Overview

This agent is designed to generate high-quality blog content and accompanying visual prompts. It follows a structured workflow to retrieve previous content (for context), generate new topics, create images, and publish across multiple platforms.

## 🚀 Get Started

[⬇️ Download agent.json](./agent.json)

> [!TIP]
> To use this agent, import the `.json` file into your n8n instance and configure the required credentials (Gemini, LinkedIn, WordPress, Google Drive).

## 📊 Workflow Diagram

```mermaid
graph TD
    Trigger([Click - Manual Trigger]) --> GetPost[WordPress - Get Last Post]
    GetPost --> GetContent[Set - Extract Title/Content]
    GetContent --> AIWriter[LangChain Agent - Create Content]
    
    subgraph "AI Content Generation"
        AIWriter -- "uses" --> GeminiWriter[Google Gemini - Writer]
        AIWriter -- "uses" --> Memory[Memory Buffer Window]
        AIWriter -- "uses" --> OutputParser[Structured Output Parser]
    end

    AIWriter --> ClearContent[Code - Sanitize Markdown]
    AIWriter --> Designer[Google Gemini - Generate Image]

    ClearContent --> CreateBlog[WordPress - Create Draft Post]
    ClearContent --> CreatePost[LinkedIn - Publish Post]
    Designer --> UploadDrive[Google Drive - Upload Banner]

    style AIWriter fill:#f9f,stroke:#333,stroke-width:2px
    style GeminiWriter fill:#fff,stroke:#333,dash-array: 5 5
```

## 🛠️ Interoperability

This agent is built to be "Agent-Friendly". You can trigger it from another agent using:
- **HTTP Node**: Call the n8n Webhook URL (if configured).
- **Execute Workflow Node**: Call this workflow by its ID.

## 📋 Metadata

- **Author:** [Ilkay Beydah Saglam](https://github.com/beydah)
- **Created Date:** 2025-10-20
- **Last Updated:** 2026-02-20
- **Category:** Content Automation / AI Agents

---
[🔙 Back to Main README](../../README.md)
