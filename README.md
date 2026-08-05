## Harrison C. Songolo

**Backend and application-security engineer. I build multi-tenant SaaS on PostgreSQL, then attack it.**

Authentication, tenant isolation, subscription webhooks, and the database rules that stop a
client from granting itself a plan. Every control I claim has a test that executes the
exploit and asserts it fails — and I check those tests by deleting the control and watching
them catch it. Two production platforms, both mine end to end, plus native C++ audio work
when a problem needs it.

TypeScript · Python · Node · SQL · PostgreSQL · Supabase · Stripe · FastAPI · React
OWASP Top 10 · row-level security · threat modeling · adversarial test suites · SIEM events

Portfolio: **[xkaii.studio/work](https://xkaii.studio/work)** · Live product: **[th3circle.app](https://th3circle.app)**

---

### Security work, mapped to OWASP

| OWASP 2021 | Attack executed in a test | Where |
|---|---|---|
| **A01** Broken Access Control | Cross-tenant read, write, delete; self-granted admin; tenant reassignment; cross-creator file upload; path traversal; null-byte injection | tenant-isolation · production suite |
| **A02** Cryptographic Failures | Tampered tracking token; token replay across keys | production suite |
| **A03** Injection | Shadow a table in `pg_temp` so a `security definer` function reads the attacker's row | tenant-isolation |
| **A04** Insecure Design | Bypass the free-tier limit by calling the API instead of the UI; defeat progressive lockout | both |
| **A05** Security Misconfiguration | Disable the protection trigger from an ordinary session; CORS from an unknown origin; localhost accepted in production | both |
| **A07** Auth Failures | Missing and forged tokens; tampered OAuth state; spoofed session fingerprint | production suite |
| **A08** Data Integrity Failures | Replay a Stripe webhook to double-apply an upgrade; client-supplied `from` header spoofing | both |
| **A09** Logging Failures | Assert auth failures and rate-limit violations emit structured SIEM events | production suite |

**tenant-isolation** is
[public and runnable](https://github.com/Th3Circle-app/tenant-isolation-postgres) — 16
integration tests against real Postgres in Docker, `npm run verify`. The **production
suite** is the closed-source pentest layer on [th3circle.app](https://th3circle.app): 30+
adversarial cases across auth, upload, CORS, rate limiting, crypto and audit logging,
running in CI on every push.

I test the tests. Removing a control has to turn a suite red, or the suite was decorating.

---

### Two security decisions I would ask me about

Both are extracted, runnable, and tested in
**[tenant-isolation-postgres](https://github.com/Th3Circle-app/tenant-isolation-postgres)**:
16 integration tests that execute each attack against real Postgres and assert it fails.


**Row-level security gates rows, not columns.**
On [th3circle.app](https://th3circle.app) I found that a tenant could read rows correctly
and still write to columns they had no business touching, which is a privilege-escalation
path RLS alone does not close. The fix was layered rather than clever: RLS policies for row
scope, an API-level column allowlist so the surface never accepts the field at all, and a
database trigger that blocks writes to privileged columns regardless of who is asking.
Three levels, because any one of them can be bypassed by a route I forget about later.

**Never let the client meter itself.**
In [rollout](https://github.com/Th3Circle-app/rollout), free-tier limits live in Postgres,
not the browser. `security definer` functions plus a trigger mean the client can call
anything it likes and still cannot grant itself a plan or reset its own usage counter. If
your billing logic is enforceable from devtools, you do not have billing logic.

---

### What I have shipped

**[redteam-loop](https://github.com/Th3Circle-app/redteam-loop)** · Node, OWASP, Claude
An automated red-team loop: it fires OWASP-classified attacks at a running service, triages
what lands, has Claude propose a minimal patch, and then **verifies the patch actually
closes the hole** by re-running the exact attack against the patched code before a human
sees it. The machine detects, classifies, proposes, and verifies; a person merges. The
generalized version of the security testing on th3circle.app — the honest AI DevSecOps loop,
not the "autonomously patches prod" fantasy no serious team would run.

**[th3circle.app](https://th3circle.app)** · live production · [architecture writeup](https://github.com/Th3Circle-app/th3circle-architecture)
Multi-creator subscription platform. Stripe checkout, subscriptions, Connect payouts, and
webhook-driven fulfillment on Node serverless functions, over a Postgres schema on
row-level security. Passwordless OTP auth, a Twilio SMS broadcast system, cron-scheduled
email sequences with paced bulk sending, and a PWA with silent service-worker updates.
The full system design — request flow, component map, and the OWASP-mapped security model
— is documented in [th3circle-architecture](https://github.com/Th3Circle-app/th3circle-architecture).

**[rollout](https://github.com/Th3Circle-app/rollout)** · React 19, TypeScript, FastAPI, Postgres
Multi-tenant release platform with Stripe subscriptions and database-enforced plan gating.
The Python side is a real ML pipeline: calibrated zero-shot classification on LAION-CLAP,
and forced lyric alignment that treats the ASR transcript as a locator rather than as
truth, so it returns nothing instead of hallucinating on an instrumental.

**[backline](https://github.com/Th3Circle-app/backline)** · C++, JUCE, macOS
A native music-to-picture editor: bus routing with plugin delay compensation, BS.1770 /
R128 loudness metering with normalized export and stems, and AVFoundation video playback.
Written against a real-time audio callback, where there is no garbage collector and the
arithmetic has to be right. Here because it is the clearest evidence I can work far below
the framework layer when a problem requires it.

**[talent-finder](https://github.com/Th3Circle-app/talent-finder)** · [live tool](https://th3circle-app.github.io/talent-finder/) · vanilla JS, zero dependencies
Built from a problem I lived doing A&R: leads pile up in a spreadsheet that cannot play a
track or remember who you already vetted. Import a CSV and it becomes a working surface,
with real Spotify, YouTube, Apple Music and SoundCloud players opening inline in each card.
One HTML file, no build step, runs offline by double-clicking, nothing leaves the browser.
The parsing and link-detection core is factored out into a module with a `node:test` suite
that runs in CI on every push.

**Internal tooling at a music production house** · built with their engineering team
Hired into a production role, I audited the company's operational infrastructure and
rebuilt artist intake end to end. The old funnel was an unbranded Google Form that
prospects mistrusted, so I replaced it with a branded portal carrying per-team-member
attribution links, meaning every submission traces back to whoever sourced it. Behind it,
a Google Apps Script pipeline: submissions land in a shared work pool, a reviewer claims
an artist, the record moves into audit for grading against genre-specific rubrics, and
completed records export straight into the main database instead of being copy-pasted
between sheets.

I worked **inside their existing codebase alongside their developer** to extend the
internal operations workstation the team now runs on daily. The interesting part was not
the code, it was mapping how the team actually worked before deciding what to automate.

---

### How I work

Generated code is a draft. The schema, the trust boundary, the failure modes, and the
question of whether a thing should exist at all are mine. Ask me about any decision in
these repos and I will tell you what the alternatives were and why I ruled them out.

One example of the judgment part. My mood classifier once labeled an artist's
self-discovery song "romantic." Defensible from the audio, completely wrong for how he
would market it. I added a negative prior on presentation-risky labels so they have to
strongly dominate before surfacing, because being confidently wrong in a user's face is
worse than being vague. The model had no way to know that. That call is the job.

Before software I was a union digital imaging technician (IATSE Local 481) on Netflix
features, where a deadline is a physical fact and the work has to be right the first time.

**[xkaii.studio](https://xkaii.studio)** · [LinkedIn](https://www.linkedin.com/in/harrison-songolo-27683670/) · harrison@xkaii.com
