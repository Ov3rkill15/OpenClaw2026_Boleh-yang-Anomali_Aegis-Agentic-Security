# 👥 Aegis Agent Coordination & Standards

This document outlines the responsibilities and collaboration rules for the Aegis development team and autonomous agents.

## 📋 Agent Responsibilities & Assignments

| Agent/Module | Owner | Core Responsibility |
| :--- | :--- | :--- |
| **Recon Agent** | @agent_recon | Project mapping & stack detection logic. |
| **Security Agent** | @agent_security | SAST engine and vulnerability pattern matching. |
| **Pentest Agent** | @agent_pentest | DAST orchestration and sandbox exploitation. |
| **Patch Agent** | @agent_patch | AI surgical patching and code transformation. |
| **Validation Agent** | @agent_validation | Patch verification and regression testing. |
| **Frontend/Teaser** | @agent_frontend | Landing page, dashboard, and visual UI. |

## 🤝 Collaboration Rules
- **Modular Ownership:** No agent should modify the core logic of another agent's module without a peer review.
- **Shared Context:** All data shared between agents must follow the `AegisContext` schema defined in OpenClaw.
- **Git Workflow:**
  - Main branch is for production-ready code only.
  - Feature branches: `feat/<agent-name>/<task>`.
  - Fix branches: `fix/<issue-id>`.

## 🛠️ Coding Standards
- **CLI-First:** Every feature must be accessible via the terminal.
- **Aesthetics:** Use the established color palette (Red, Cyan, Gray) for all console outputs.
- **Resilience:** Every agent must handle network failures and AI timeouts gracefully (Retry logic).
- **Security:** Do not log sensitive data (API keys, etc.) in plain text; use masking.

## 📈 Progress Tracking
Every developer/agent MUST maintain their own progress file in the `Aegisfix` folder. These files are **APPEND-ONLY**.

### Progress Files:
- `progres_recon.md`
- `progres_security.md`
- `progres_pentest.md`
- `progres_patch.md`
- `progres_validation.md`
- `progres_frontend.md`

### Entry Format:
```markdown
### [YYYY-MM-DD HH:MM] - Task Name
- **Task Completed:** Description of what was done.
- **Files Changed:** list of files.
- **Summary:** Quick technical summary.
- **Next Task:** What is planned next.
```
