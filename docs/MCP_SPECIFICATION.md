# OpenAIDLC Model Context Protocol (MCP) Tool Bus Specification

## 1. Motivation

Connecting AI agents to real-world developer tools has historically required building fragile, custom glue code for every proprietary API. 

**OpenAIDLC** eliminates custom integration glue by adopting Anthropic's **Model Context Protocol (MCP)** as its universal outer-loop tool bus. Every tool—from issue trackers to CI/CD pipelines—is represented as an open, standard MCP server that exposes:
* **Resources:** Context data (e.g., ticket descriptions, pull requests, design docs).
* **Tools:** Callable actions (e.g., creating branches, triggering builds, running tests).
* **Prompts:** Specialized prompt templates for domain tasks.

---

## 2. Standard MCP Tool Categories in OpenAIDLC

```
┌──────────────────────────────────────────────────────────────┐
│                    OpenAIDLC MCP Tool Bus                    │
├──────────────┬──────────────┬───────────────┬────────────────┤
│ Issue & PRs  │ Knowledge    │ Quality (QA)  │ Infrastructure │
├──────────────┼──────────────┼───────────────┼────────────────┤
│ • Linear     │ • Notion     │ • Playwright  │ • Docker       │
│ • GitHub     │ • Confluence │ • Appium      │ • Kubernetes   │
│ • Jira       │ • Local PRDs │ • Jest/PyTest │ • Sentry       │
└──────────────┴──────────────┴───────────────┴────────────────┘
```

---

## 3. Configuration & Registration

MCP servers are registered directly within `aidlc.config.yaml` or `.mcp.json`:

```yaml
mcp_servers:
  # Issue Management
  linear:
    command: "npx"
    args: ["-y", "@linear/mcp-server"]
    env:
      LINEAR_API_KEY: "${LINEAR_API_KEY}"

  # Code & Pull Requests
  github:
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-github"]
    env:
      GITHUB_PERSONAL_ACCESS_TOKEN: "${GITHUB_TOKEN}"

  # Knowledge & Architecture Specs
  notion:
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-notion"]
    env:
      NOTION_API_KEY: "${NOTION_API_KEY}"

  # End-to-End Web Testing
  playwright:
    command: "npx"
    args: ["-y", "@playwright/mcp-server"]

  # Mobile App Testing (iOS / Android)
  appium:
    command: "appium-mcp"
    args: ["--host", "127.0.0.1", "--port", "4723"]
```

---

## 4. MCP Security & Governance

OpenAIDLC enforces three layers of security at the MCP layer:

1. **Least-Privilege Scoping:** Tools are registered with read-only or read-write permissions. Agents cannot invoke destructive actions without passing human approval checkpoints.
2. **Execution Sandboxing:** Shell and container execution tools run in isolated sub-environments, preventing unauthorized filesystem modifications.
3. **Environment Injection Protection:** API secrets and tokens are resolved dynamically from developer environment variables and never logged or serialized into LLM prompt contexts.
