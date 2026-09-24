# Contributing to OpenAIDLC

Thank you for your interest in contributing to **OpenAIDLC**! OpenAIDLC is an open-source framework dedicated to making autonomous agentic software development life cycles cost-efficient, customizable, and safe for engineering teams.

We welcome contributions of all kinds: bug reports, documentation enhancements, feature proposals, and pull requests!

---

## 🧭 Code of Conduct

All contributors and maintainers are expected to abide by our [Code of Conduct](CODE_OF_CONDUCT.md). Please report unacceptable behavior through our repository issue tracker or maintainers' contacts.

---

## 🛠️ How to Contribute

### 1. Reporting Issues & Proposing Features
* Use GitHub Issues to submit bug reports or feature ideas.
* Before opening a new issue, search existing issues to avoid duplicates.
* For bug reports, please include:
  * OpenAIDLC version
  * Your operating system and environment
  * Minimal steps to reproduce the issue
  * Expected vs. actual behavior

### 2. Developing New Features & Fixes
1. **Fork the repository** on GitHub: `https://github.com/jmbp1999/open-aidlc`
2. **Clone your fork locally**:
   ```bash
   git clone https://github.com/<your-username>/open-aidlc.git
   cd open-aidlc
   ```
3. **Create a descriptive feature branch**:
   ```bash
   git checkout -b feat/mcp-connector-linear
   ```
4. **Make your changes**:
   * Adhere to existing code conventions.
   * Write modular, testable code.
   * Update relevant documentation in `docs/` and configuration schemas in `config/`.
5. **Commit your changes using Conventional Commits**:
   ```bash
   git commit -m "feat(mcp): add bi-directional Linear ticket synchronization"
   ```
6. **Push to your fork and submit a Pull Request**:
   * Clearly explain what problem the PR solves.
   * Link any related issues (e.g., `Closes #12`).

---

## 📐 Architecture & Contribution Focus Areas

We especially welcome contributions across the following key layers:

1. **Model Adapters (`docs/TIERED_ROUTING.md`):**
   * Connectors for local SLMs via Ollama, vLLM, and Llama.cpp.
   * Routing heuristics for emerging high-throughput models.
2. **MCP Tool Connectors (`docs/MCP_SPECIFICATION.md`):**
   * New Model Context Protocol integrations (e.g., Sentry, GitLab, Postman, Supabase).
3. **Autonomous Project Initializer (`aidlc init --ai`):**
   * Language heuristics for scanning Rust, Go, Python, TypeScript, and Java projects.
4. **DevOps & CI/CD Governance:**
   * Policy engines, pre-commit blast-radius calculators, and isolated Git worktree management.

---

## 📜 License

By contributing to OpenAIDLC, you agree that your contributions will be licensed under the project's [Apache License 2.0](LICENSE).
