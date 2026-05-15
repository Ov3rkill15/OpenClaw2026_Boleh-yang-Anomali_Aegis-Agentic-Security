# Aegis: QA Workflow & Code Quality Standards

## 1. QA Role Definition
The QA Engineer is responsible for ensuring code quality, consistency, and reliability across **all** Aegis agents and modules. This role operates as a cross-cutting concern — reviewing output from every phase of the pipeline.

### Pipeline Coverage
```
[Phase 0: Ingestion] → [Phase 1: SAST] → [Phase 2: Pentest/DAST] → [Phase 3: Self-Healing/Monitor]
       ↑                     ↑                     ↑                          ↑
    QA Gate 1             QA Gate 2             QA Gate 3                  QA Gate 4
```

---

## 2. Quality Gates

### Gate 1 — Ingestion (Recon Agent)
| Check | Criteria | Pass Condition |
|:---|:---|:---|
| Output Schema | JSON output follows OpenClaw standard | Valid JSON, all required fields present |
| File Discovery | All project files correctly mapped | No missed directories, correct tech stack detection |
| Metadata Accuracy | Tech stack, entry points, dependencies | Matches manual verification |
| Error Handling | Graceful failure on unreadable files | No crashes, proper error messages logged |

### Gate 2 — Static Analysis (Security Agent)
| Check | Criteria | Pass Condition |
|:---|:---|:---|
| Detection Accuracy | Finds known vulnerabilities in test fixtures | ≥90% true positive rate on test suite |
| False Positive Rate | Does not flag secure code as vulnerable | ≤10% false positive rate |
| Severity Classification | Correct severity tagging (Critical/High/Medium/Low) | Matches OWASP classification |
| Report Generation | AEGIS_REPORT.md is complete and accurate | All findings documented with file + line reference |

### Gate 3 — Pentest / DAST (Pentest Agent)
| Check | Criteria | Pass Condition |
|:---|:---|:---|
| Exploit Validation | Confirms reported vulnerabilities are exploitable | Matches Security Agent findings |
| Safety Boundary | No destructive actions on target system | Sandbox-only, no data mutation |
| Payload Accuracy | Correct payload selection per vulnerability type | SQLi payload for SQLi, XSS for XSS, etc. |
| Result Format | Output compatible with Patch Agent input | Valid OpenClaw JSON handover |

### Gate 4 — Self-Healing & Monitor (Patch + Validation Agent)
| Check | Criteria | Pass Condition |
|:---|:---|:---|
| Patch Correctness | Applied patch actually fixes the vulnerability | Re-scan shows vulnerability resolved |
| No Regression | Patch does not break existing functionality | Original logic preserved |
| Backup Safety | `.bak` file created before every patch | Verified `.bak` exists with original content |
| Undo Mechanism | `aegis undo` restores original file | Byte-for-byte match with pre-patch version |
| Security Score | Post-patch score improves | Score delta > 0, ideally 10/10 |

---

## 3. Code Quality Standards (All Modules)

### 3.1 Structure & Style
- [ ] Consistent file/folder naming across all agents
- [ ] Each agent has a clear entry point (`index.js` / `main.py`)
- [ ] No dead code or unused imports
- [ ] Functions are single-responsibility (max ~50 lines)
- [ ] Consistent error handling pattern (try-catch with logging)

### 3.2 Security (Eating Our Own Dog Food)
- [ ] No hardcoded secrets, API keys, or credentials in source
- [ ] All user input is sanitized before processing
- [ ] No sensitive data in console logs or progress files
- [ ] Dependencies are pinned to specific versions

### 3.3 Agent Communication
- [ ] All inter-agent data uses the OpenClaw JSON standard
- [ ] Input validation at every agent boundary
- [ ] Graceful degradation if upstream agent fails
- [ ] Handover includes timestamp, source agent ID, and payload hash

### 3.4 CLI Experience
- [ ] Every command has `--help` documentation
- [ ] Consistent color scheme across all agent outputs
- [ ] Progress indicators for long-running operations
- [ ] Exit codes are meaningful (0 = success, 1 = error, 2 = partial)

---

## 4. Testing Strategy

### 4.1 Test Fixtures (Vulnerable Sandbox)
Create a set of intentionally vulnerable test files to validate agent accuracy:

```
tests/
  fixtures/
    vulnerable/
      hardcoded_secret.js    ← Contains API_KEY = "sk-..."
      sql_injection.py       ← Uses f-string in SQL query
      xss_endpoint.js        ← Unsanitized user input in HTML
      insecure_auth.py       ← Plaintext password comparison
    secure/
      env_secret.js          ← Uses process.env.API_KEY
      parameterized_query.py ← Uses parameterized SQL
      sanitized_output.js    ← HTML-escaped user input
      hashed_auth.py         ← bcrypt password comparison
```

### 4.2 Test Types
| Type | What It Validates | When To Run |
|:---|:---|:---|
| **Unit Tests** | Individual functions within each agent | Every commit |
| **Integration Tests** | Agent-to-agent handover (JSON contract) | Before merge to main |
| **End-to-End Tests** | Full `aegis autopilot .` pipeline on sandbox | Before demo/submission |
| **Regression Tests** | Patches don't break existing functionality | After every patch cycle |

### 4.3 Smoke Test Command
```bash
# Quick validation of the entire pipeline
aegis autopilot ./tests/fixtures/vulnerable/ --dry-run
```
Expected: Detects all 4 vulnerabilities, generates patches, verifies fixes.

---

## 5. QA Review Process

### Per-Agent Review Checklist
When reviewing any agent's code, verify:

```markdown
## Agent Review: [Agent Name]
- [ ] Entry point is clear and documented
- [ ] Input schema is validated
- [ ] Output matches OpenClaw JSON contract
- [ ] Error states are handled gracefully
- [ ] Logging follows cinematic UX standard (colors, format)
- [ ] No hardcoded values — configurable via CLI flags or config
- [ ] Progress file updated with real, specific entries
- [ ] Code comments explain "why", not "what"
```

### Cross-Agent Review
```markdown
## Pipeline Integration Review
- [ ] Recon → Security handover: metadata format matches
- [ ] Security → Pentest handover: vulnerability format matches  
- [ ] Pentest → Patch handover: confirmed vulns format matches
- [ ] Patch → Validation handover: patch manifest format matches
- [ ] Full pipeline runs without manual intervention
```

---

## 6. Documentation QA

### Consistency Checks
- [ ] All planning docs (plan.md, feature.md, agent.md) are in sync
- [ ] Agent count matches across all documents
- [ ] Folder references match actual repository structure
- [ ] Progress files follow the template defined in agent.md
- [ ] No broken emoji or encoding issues in any file
- [ ] README.md accurately reflects current project state

---

## 7. Hackathon Judging Alignment

Based on OpenClaw 2026 judging criteria, QA must specifically verify:

| Judging Criteria | QA Checkpoint |
|:---|:---|
| **Agentic Architecture & Autonomy** | Agents make decisions autonomously, not hard-coded if/else |
| **Security & Access Control** | Aegis itself follows security best practices |
| **Engineering Quality** | Clean architecture, error handling, consistent patterns |
| **Robustness** | Pipeline handles edge cases without crashing |
| **Real-World Viability** | Could run on a real project, not just demo fixtures |

---

*Last Updated: 2026-05-15 10:27 (QA Engineer)*
