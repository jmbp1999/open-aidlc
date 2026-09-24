# OpenAIDLC System Architecture & Core Engine

## 1. Overview

**OpenAIDLC** provides a modular, extensible, and cost-efficient architecture for operationalizing autonomous AI agents across the entire Software Development Life Cycle (SDLC).

Rather than binding developers to a single model provider or monolithic execution sandbox, OpenAIDLC establishes a **6-layer modular architecture** centered on declarative configuration (`aidlc.config.yaml`), the **Model Context Protocol (MCP)**, and **Cost-Aware Tiered Model Routing**.

```
┌────────────────────────────────────────────────────────┐
│ Layer 6: Autonomous AI Setup Agent (aidlc init --ai)   │
├────────────────────────────────────────────────────────┤
│ Layer 5: Multi-Runtime Adapters (Antigravity/Claude/CI)│
├────────────────────────────────────────────────────────┤
│ Layer 4: Policy-as-Code Governance & Spending Caps     │
├────────────────────────────────────────────────────────┤
│ Layer 3: Composable Workflow-as-Code (DAG Engine)      │
├────────────────────────────────────────────────────────┤
│ Layer 2: Universal Tool Bus (Model Context Protocol)   │
├────────────────────────────────────────────────────────┤
│ Layer 1: Pluggable Model Routing (BYOM Tiered Engine)  │
└────────────────────────────────────────────────────────┘
```

---

## 2. The 6 Architectural Layers

### Layer 1: Pluggable Model Routing (BYOM Tiered Engine)
* **Design Rationale:** Most coding agents burn $10–$25+ per task by sending every routine file read, AST parse, and test output to expensive frontier LLMs.
* **Mechanism:** OpenAIDLC introduces a three-tier execution hierarchy:
  * **Tier 0 ($0.00 / Free Local SLMs):** Local models (DeepSeek-Coder 7B, CodeLlama via Ollama/vLLM) and Tree-sitter parsers handle AST slicing, lint checking, syntax validation, and diff formatting at **zero cost**.
  * **Tier 1 (Sub-Cent High-Speed Models):** Gemini 1.5 Flash or Claude 3.5 Haiku handle issue triage, PR summaries, and build log compaction.
  * **Tier 2 (Frontier LLMs):** Claude 3.5 Sonnet or GPT-4o are reserved exclusively for architectural design and complex multi-file logic synthesis.

### Layer 2: Universal Tool Bus (Model Context Protocol - MCP)
* **Design Rationale:** Hardcoding API clients for dozens of developer tools results in massive maintenance debt and fragile integrations.
* **Mechanism:** OpenAIDLC implements Anthropic's open **Model Context Protocol (MCP)** specification as its universal tool bus. All external systems—issue trackers (Linear, Jira), knowledge bases (Notion, Confluence), communication (Slack, Teams), test runners (Playwright, Appium), and environments (Docker, Kubernetes)—are consumed as declarative MCP servers with zero custom glue code.

### Layer 3: Composable Workflow-as-Code (DAG Engine)
* **Design Rationale:** Software engineering is not a linear conversation; it consists of conditional branching, parallel executions, and fallback strategies.
* **Mechanism:** OpenAIDLC models SDLC tasks as Directed Acyclic Graphs (DAGs) defined in YAML:
  * **L1 Support Triage:** Customer tickets parsed, reproduced, and routed.
  * **L2 Dev-Fix:** Bug localization, solution synthesis, patch application, and unit verification.
  * **L3 Architecture & RAG:** Querying system PRDs, calculating blast radius, and validating cross-service dependencies.
  * **QA Automation:** Generating end-to-end browser and mobile tests.

### Layer 4: Policy-as-Code Governance & Worktree Safety
* **Design Rationale:** Unsupervised AI agents can destroy git branches, introduce hallucinated dependencies, or incur runaway API bills.
* **Mechanism:**
  * **Isolated Git Worktrees:** OpenAIDLC always executes code changes inside isolated Git worktrees (`git worktree add`), keeping the active developer branch pristine until all tests pass.
  * **Hard Spending Caps:** A configurable `max_cost_usd_per_task: 0.50` parameter terminates or downgrades agent loops before financial runaway occurs.
  * **Human Approval Gates:** Configurable pause breakpoints (`pause_for_human_review: true`) require explicit developer confirmation before executing code or pushing branches.

### Layer 5: Multi-Runtime Adapters
* **Design Rationale:** Teams use diverse developer interfaces—some prefer CLI terminals, others IDE sidecars, others CI/CD automation.
* **Mechanism:** OpenAIDLC compiles its unified configuration into runtime-specific instruction harnesses:
  * Google Antigravity (`AGENTS.md`)
  * Anthropic Claude Code (`CLAUDE.md`)
  * Cursor (`.cursorrules`)
  * GitHub Actions / GitLab CI pipelines

### Layer 6: Autonomous AI Setup Agent (`aidlc init --ai`)
* **Design Rationale:** Setting up agent environments manually takes days of reading schemas and configuring permissions.
* **Mechanism:** A turnkey onboarding agent:
  1. Inspects the repository filesystem and fingerprints package managers, test runners, and build scripts.
  2. Conducts a rapid 3-question alignment interview to determine governance policy and tool stack.
  3. Scaffolds `aidlc.config.yaml`, `.mcp.json`, and runtime harnesses automatically in minutes.
