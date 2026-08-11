## Harrison C. Songolo

**AI Security Engineer — I build software the AI-native way, then attack what I build.**

I ship production systems with AI as my primary tool, then red-team them: every control I claim
has a test that fires the real exploit and asserts the system refuses it. My focus is the
security of AI-driven software and the tools we hand to AI agents.

### Proven, not packaged

- **[th3circle.app](https://th3circle.app) runs in production.** A multi-tenant subscription
  platform I built end to end — 64 serverless functions, PostgreSQL row-level security, Stripe
  payments, webhook-driven fulfillment. Not a demo, a live product.
- **Real vulnerabilities, responsibly disclosed.** I found, fixed, and disclosed two Server-Side
  Request Forgery bugs (CWE-918) in open-source tools. On
  [neonlink](https://github.com/AlexSciFier/neonlink) the maintainer **merged my security-policy
  PR and accepted my private advisory** — third-party validation you can't manufacture.

### What I do → the repos

- **[security-assessments](https://github.com/Th3Circle-app/security-assessments)** — real
  application-security assessments of live open-source apps: two confirmed SSRFs found → fixed →
  verified against a running instance and disclosed, plus three documented *non-findings* — the
  judgment calls that show how I decide when something *isn't* a bug, and why.
- **[provekit-mcp](https://github.com/Th3Circle-app/provekit-mcp)** — a hardened
  [MCP](https://modelcontextprotocol.io) server that gives an AI agent a code-security scanner,
  built so the tools can't be turned against the host, plus a red-team suite that spawns the real
  server and attacks it over the protocol. **9 attacks held, 0 breaches, 98 tests** including the
  live red-team. Full threat model in the README.
- **[redteam-loop](https://github.com/Th3Circle-app/redteam-loop)** — the methodology behind all
  of it: an automated **attack → propose-fix → re-fire-the-exact-attack → verify** loop that
  proves a fix actually closes the hole before a human merges it.
- **[appsec-rag](https://github.com/Th3Circle-app/appsec-rag) & [langchain-appsec-agent](https://github.com/Th3Circle-app/langchain-appsec-agent)**
  — that loop turned on my *own* AI tools. Two Python security assistants (a cited RAG and a
  LangChain agent), red-teamed round after round until two consecutive passes found nothing:
  **11 rounds, 10 real bugs, each fixed with a regression test**. Findings per round:
  `2 → 4 → 1 → 1 → 0 → 1 → 0 → 1 → 1 → 0 → 0`. The middle rounds mattered most — they caught the
  false-positives, the bugs that don't crash and pass a casual test run, they just quietly lie.

### See it run

Paste code into the **[live scanner at xkaii.studio/labs/scanner](https://xkaii.studio/labs/scanner)**
(runs 100% in your browser) · security work at
**[xkaii.studio/security](https://xkaii.studio/security)**

---

`Python` · `TypeScript` · `Node` · `PostgreSQL` · `MCP` · OWASP Top 10 · DAST / SAST ·
LLM & agent trust boundaries · threat modeling · adversarial test suites · DevSecOps / AppSec
