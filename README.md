## Harrison C. Songolo

**Backend engineer specializing in multi-tenant SaaS: PostgreSQL, row-level security, and Stripe billing.**

I build and operate the parts that break at 2am. Authentication, tenant isolation,
subscription webhooks, and the database rules that stop a client from granting itself a
plan. Two production platforms, both mine end to end, plus native C++ audio work when a
problem needs it.

TypeScript · Python · Node · SQL · PostgreSQL · Supabase · Stripe · FastAPI · React

Portfolio: **[xkaii.studio/work](https://xkaii.studio/work)** · Live product: **[th3circle.app](https://th3circle.app)**

---

### Two security decisions I would ask me about

Both are extracted, runnable, and tested in
**[tenant-isolation-postgres](https://github.com/Th3Circle-app/tenant-isolation-postgres)**:
15 integration tests that execute each attack against real Postgres and assert it fails.


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

**[th3circle.app](https://th3circle.app)** · live production
Multi-creator subscription platform. Stripe checkout, subscriptions, Connect payouts, and
webhook-driven fulfillment on Node serverless functions, over a Postgres schema on
row-level security. Passwordless OTP auth, a Twilio SMS broadcast system, cron-scheduled
email sequences with paced bulk sending, and a PWA with silent service-worker updates.

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
question of whether a thing should exist at all are mine, and I can walk through any line
of these repos and tell you why it is there.

One example of the judgment part. My mood classifier once labeled an artist's
self-discovery song "romantic." Defensible from the audio, completely wrong for how he
would market it. I added a negative prior on presentation-risky labels so they have to
strongly dominate before surfacing, because being confidently wrong in a user's face is
worse than being vague. The model had no way to know that. That call is the job.

Before software I was a union digital imaging technician (IATSE Local 481) on Netflix
features, where a deadline is a physical fact and the work has to be right the first time.

**[xkaii.studio](https://xkaii.studio)** · [LinkedIn](https://www.linkedin.com/in/harrison-songolo-27683670/) · harrison@xkaii.com
