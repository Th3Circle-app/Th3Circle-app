## Harrison C. Songolo

**AI product engineer specializing in audio and media systems.**

I build software end to end: the model pipeline, the API, the interface, and the
deploy. Most of my work lives where machine learning meets real-time media, which is
the part that does not come out of a framework.

Based in Northern California. Portfolio: **[xkaii.studio/work](https://xkaii.studio/work)**

---

### What I am working on

**[rollout](https://github.com/Th3Circle-app/rollout)** · React 19, TypeScript, FastAPI, Postgres
Drop one song, get a whole release. A multi-tenant platform that listens to a track and
builds the release kit around what it hears.
The interesting parts: calibrated zero-shot mood classification on LAION-CLAP (prompt
ensembles, z-score normalization across labels, five-window listening, music-theory
fusion), forced lyric alignment that treats the ASR transcript as a locator rather than
as truth so the artist's own words always win, a canvas layer compositor where the
preview and the 3000px export call the same function, and free-tier metering enforced by
`security definer` functions in the database instead of by the client.

**[backline](https://github.com/Th3Circle-app/backline)** · C++, JUCE, macOS
A native music-to-picture editor: a hybrid video editor and DAW. Bus routing with plugin
delay compensation, BS.1770 / EBU R128 loudness metering with normalized export and
stems, semitone pitch shifting, and AVFoundation video playback. Ships as a DMG.
Written against a real-time audio callback, where there is no garbage collector to save
you and the arithmetic has to be right.

**[th3circle.app](https://th3circle.app)** · live product
A multi-creator subscription platform I designed, built, and operate. Stripe checkout,
subscriptions, Connect payouts, webhook-driven fulfillment, passwordless auth, and a
Postgres schema on row-level security. While building it I found that RLS gates rows but
not columns, and closed the privilege-escalation gap with a layered fix: policies, an
API-level column allowlist, and a trigger blocking writes to privileged columns.

---

### How I work

I am fast because I am deliberate about which decisions are mine and which are the
machine's. Generated code is a draft. The architecture, the failure modes, the security
boundary, and the question of whether a thing should exist at all are mine, and I can
walk through any line in these repos and tell you why it is there.

A concrete example. My mood classifier once labeled an artist's self-discovery song
"romantic." It was defensible from the audio and completely wrong for how he would market
it, so I added a negative prior on presentation-risky labels: they now have to strongly
dominate before they surface. Being confidently wrong in a user's face is worse than
being vague. That is the kind of judgment the model does not have.

---

### Stack

```
Languages    TypeScript · Python · C++ · SQL
AI / Audio   LAION-CLAP · demucs · stable-ts · librosa · Remotion · JUCE · SoundTouch
Frontend     React · Vite · Tailwind · canvas rendering
Backend      FastAPI · Node · Postgres (RLS, triggers, security definer) · Supabase
Payments     Stripe (subscriptions, Connect, webhooks)
Delivery     Netlify · CI/CD · Git
```

Before software I was a union digital imaging technician (IATSE Local 481) on Netflix
features, which is where I learned that a deadline is a physical fact and the work has to
be right the first time.

**[xkaii.studio](https://xkaii.studio)** · [LinkedIn](https://www.linkedin.com/in/harrison-songolo-27683670/) · harrison@xkaii.com
