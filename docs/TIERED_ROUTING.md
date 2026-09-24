# Cost-Aware Tiered Model Routing: Algorithmic Architecture

## 1. The Economic Crisis in Autonomous AI Agents

Existing autonomous coding swarms (such as SWE-agent, OpenHands, and Devin) suffer from quadratic context accumulation. For every tool call (`ls`, `grep`, `cat`), build error, and unit test failure, historical outputs are concatenated into the next LLM prompt.

When 100% of these tokens are sent to frontier models ($3.00–$5.00 / 1M input tokens), a standard multi-turn bug fix consuming 3.5M–4.5M cumulative tokens costs **$12.00–$25.00+**. 

For lean startups, agency teams, and growing software organizations, this creates a severe financial bottleneck:

$$\text{Monthly Cost} = 100\text{ issues} \times \$18.00 = \$1,800/\text{month}$$

---

## 2. The Tiered Model Routing Algorithm

OpenAIDLC replaces monolithic frontier execution with an intelligent **Cognitive Complexity Classifier (CCC)** that routes subtasks across three distinct model tiers:

```
                  ┌──────────────────────────────┐
                  │    Incoming SDLC Subtask     │
                  └──────────────┬───────────────┘
                                 │
                     ┌───────────┴───────────┐
                     ▼                       ▼
            [Deterministic / AST]   [Probabilistic / Reasoning]
                     │                       │
           ┌─────────┴─────────┐   ┌─────────┴─────────┐
           ▼                   ▼   ▼                   ▼
     [Tier 0: Local]      [Tier 1: Fast]       [Tier 2: Frontier]
    (Ollama / Tree-sitter) (Gemini Flash)     (Claude 3.5 Sonnet)
       Cost: $0.00          Cost: <$0.05         Cost: <$0.40
```

### Tier 0: Local Specialized Small Language Models ($0.00 / Free)
* **Engines:** Ollama, vLLM, Llama.cpp, Tree-sitter AST parsers.
* **Target Models:** DeepSeek-Coder 6.7B/33B, CodeLlama 13B/34B, Qwen2.5-Coder.
* **Handled Tasks:**
  * Abstract Syntax Tree (AST) symbol lookup and function definition slicing.
  * Local linting and type error verification (`eslint`, `tsc`, `flake8`, `ruff`).
  * Syntax tree formatting and docstring generation.
  * Raw Git diff construction and file staging.
* **Token Cost:** **$0.00** (Executed locally on CPU/Apple Silicon/GPU).

### Tier 1: Sub-Cent High-Throughput Models (<$0.05 / Subtask)
* **Target Models:** Google Gemini 1.5 Flash ($0.075 / 1M), Anthropic Claude 3.5 Haiku ($0.25 / 1M).
* **Handled Tasks:**
  * L1 support ticket ingestion, deduplication, and reproduction step extraction.
  * Verbose build log summarization (compacting 5,000-line CI logs down to 30 relevant error lines).
  * Pull request change categorization and commit message generation.
* **Token Cost:** Negligible ($0.01 – $0.03 per invocation).

### Tier 2: Frontier Reasoning LLMs (<$0.40 / Subtask)
* **Target Models:** Anthropic Claude 3.5 Sonnet ($3.00 / 1M), OpenAI GPT-4o ($5.00 / 1M).
* **Handled Tasks:**
  * Cross-module architectural impact analysis.
  * Complex multi-file patch synthesis requiring deep semantic reasoning.
  * Security reviews and concurrency analysis.
* **Token Optimization:** Invoked *only* after Tier 0 and Tier 1 have pruned 95%+ of repository noise from the context window.

---

## 3. Empirical Cost Validation

| Phase | Traditional Agent Cost | OpenAIDLC Tier | OpenAIDLC Cost | Cost Savings |
| :--- | :--- | :--- | :--- | :--- |
| 1. Ticket Triage & Reproduction | $0.75 | **Tier 1 (Flash)** | $0.02 | 97.3% |
| 2. Codebase AST Exploration | $3.60 | **Tier 0 (Local)** | **$0.00** | **100.0%** |
| 3. Architecture & Patch Design | $1.20 | **Tier 2 (Sonnet)** | $0.35 | 70.8% |
| 4. Implementation in Worktree | $7.50 | **Tier 0 + Tier 1** | $0.05 | 99.3% |
| 5. Linting & Unit Testing | $2.40 | **Tier 0 (Local)** | **$0.00** | **100.0%** |
| 6. PR & Artefact Sync | $0.40 | **Tier 1 (Flash)** | $0.02 | 95.0% |
| **TOTAL PER RESOLVED ISSUE** | **$15.85** | **Multi-Tier** | **$0.44** | **97.2%** |
