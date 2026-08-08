# omnigate-free

The one-command way to try **OmniGate**, the free edition — a multi-protocol database gateway
that speaks real Oracle Net, real Postgres wire protocol, and real MySQL wire protocol itself.

This repo is deliberately just the deployment package (a `docker-compose.yml`, a `.env`, and
docs) — not the source code. For the full source, architecture docs, and the commercial
(unrestricted) edition, see **[kumarrajamani-ai/Omnigate](https://github.com/kumarrajamani-ai/Omnigate)**.

**Free edition limits**: 100 concurrent connections, 2 named backends. Everything else — every
protocol, every feature — is the same code as the commercial edition; nothing is feature-gated,
only scale-gated. See the main repo's `ARCHITECTURE.md` §9 for exactly how that cap is enforced
(baked into the image itself, not just an env var default, so it can't be casually overridden).

## Quick start

```bash
git clone https://github.com/kumarrajamani-ai/omnigate-free.git
cd omnigate-free
docker compose up
```

Then open **http://localhost:8080/console** — it opens straight to a step-by-step "Get Started"
guide. Full walkthrough (three different SQL clients, one bundled backend, plus how to turn on
the optional local AI assistant) is in **[SKILLS.md](SKILLS.md)**.

## What's in here

- `docker-compose.yml` — pulls the published `ghcr.io/kumarrajamani-ai/omnigate-free:latest`
  image and a seeded Postgres backend. Doesn't build from source.
- `.env` — demo defaults (safe to commit — not real secrets). Change before exposing this beyond
  your own machine.
- `docker/init-scott.sql` — seeds the classic `scott.emp`/`scott.dept` demo tables.
- `SKILLS.md` — the actual getting-started walkthrough.
