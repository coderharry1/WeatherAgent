## ⚙️ Architecture: How It Works

The Watsonx Orchestrate Weather Agent follows a robust, auditable flow designed for enterprise-grade transparency and control:

<div align="center">

```
+-----------------+           +----------------------+           +----------------------+
|     Prompt      |  ----->   |  Orchestrator / LLM  |  ----->   |      API Calls       |
|  (user intent)  |           | (routing + planning) |           | (services, tools)    |
+-----------------+           +----------------------+           +----------------------+
         |                               |                                  |
         |                               |                                  v
         |                               |                         +---------------------+
         |                               |                         |   Data Sources      |
         |                               |                         |  (DBs, SaaS, etc.)  |
         |                               |                         +---------------------+
         |                               |                                  |
         |                               v                                  |
         |                   +----------------------+                       |
         |                   |  Audit & Governance  |  <--------------------+
         |                   |  (policies, logs,    |         Logs of
         |                   |   approvals, trace)  |         requests/results
         |                   +----------------------+ 
         |                               |
         v                               v
+-----------------+           +----------------------+
|  Human Answers  |  <----    |  Review UI / Human  |
| (final output)  |           |  in the loop (opt)  |
+-----------------+           +----------------------+
```

</div>

**Legend:**
- Every step emits structured logs into **Audit & Governance** for traceability.
- A **Human-in-the-Loop** can approve or edit before the final answer.

---

This architecture ensures:
- **Full traceability** — every API call and LLM decision is logged.
- **Enterprise auditability** — all actions are governed and reviewable.
- **Human review** — optional approval for maximum safety and compliance.

---
