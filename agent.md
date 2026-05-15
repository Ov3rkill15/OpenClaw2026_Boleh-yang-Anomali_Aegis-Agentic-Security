# Aegis: Agent Workflow & Collaboration Standards

## 1. Team & Module Ownership

| Module | Primary Owner | Agent Equivalent |
| :--- | :--- | :--- |
| **Orchestration Core** | Lead Architect | Orchestrator Agent |
| **Project Ingestion** | DevOps Engineer | Recon Agent |
| **Security Scanning** | Security Engineer | Security Agent |
| **Remediation Engine** | Backend Developer | Patch Agent |
| **Pentest Logic** | QA/Security Lead | Pentest Agent |
| **Frontend/Teaser** | UI/UX Designer | Frontend Agent |
| **Code Quality & QA** | QA Engineer | QA Workflow (cross-cutting) |

---

## 2. Collaboration Rules
- **Modular Development:** Each agent/module must be developed in its respective directory under `core/` or `agents/`.
- **Git Workflow:**
  - **Branching:** `feature/agent-name` for new developments.
  - **Commits:** Clear, descriptive messages (e.g., `feat(patch): add parameterized query support`).
- **Orchestration:** All agents must communicate through the OpenClaw standard output/JSON format to ensure seamless handovers.

---

## 3. Coding Standards
- **CLI-First:** Every feature must be accessible via terminal command.
- **Cinematic UX:** Use consistent coloring and logging patterns across all agents.
- **Security-Native:** All internal communication must be sanitized; no sensitive data in logs.

---

## 4. Progress Tracking (Persistent Logs)
Each team member/agent must maintain a persistent progress file in the `Aegisfix` folder. These files **MUST ALWAYS APPEND** new entries and **NEVER** overwrite previous history.

### Required Progress Files:
- `progres_recon.md`
- `progres_security.md`
- `progres_pentest.md`
- `progres_patch.md`
- `progres_validation.md`
- `progres_frontend.md`
- `progres_qa.md`

### Entry Format:
```markdown
## [TIMESTAMP]
- **Tasks Completed:**
  - Task 1
  - Task 2
- **Files Changed:**
  - path/to/file.py
- **Summary:** Quick summary of progress.
- **Blockers:** Any issues encountered.
- **Next Tasks:** What's planned next.
```

---
*Last Updated: 2026-05-15 10:27 (QA Engineer — added QA role & workflow)*
