# Try OmniGate (free edition) in under a minute

OmniGate is a multi-protocol database gateway: it speaks real Oracle Net, real Postgres wire
protocol, and real MySQL wire protocol *itself* — three different SQL dialects, one running
process, one bundled backend. Point three completely different database clients at the same
container and watch the same data come back through each one.

This is the **free edition**: 100 concurrent connections, 2 named backends. Same code, same
protocols as the commercial edition — only scale is capped, nothing is feature-gated.

## 1. Start it

```bash
docker compose up
```

That pulls the published `ghcr.io/kumarrajamani-ai/omnigate-free:latest` image (no build step)
and starts a seeded Postgres backend (`docker/init-scott.sql` — the classic `scott.emp` /
`scott.dept` demo tables) behind it. Give it ~20–30s on first run, then open
**http://localhost:8080/console** — it opens straight to a **Get Started** tab with the same
snippets below, live connection details, and a query box, so you don't need this file open at all
once it's running.

## 2. Connect with any of three clients

All three below hit the **same** OmniGate container, on three different ports, in three different
SQL dialects — no local install required, each uses a throwaway Docker container for the client
too.

**Postgres (`psql`) — port 5433**
```bash
docker run --rm -it postgres psql "postgresql://scott:tiger@host.docker.internal:5433/postgres"
```
```sql
SELECT * FROM scott.emp WHERE deptno = 10;
```

**MySQL (`mysql` CLI) — port 3306**
```bash
docker run --rm -it mariadb mysql -h host.docker.internal -P 3306 -u scott -ptiger postgres
```
```sql
SELECT * FROM scott.emp WHERE deptno = 10;
```

**Oracle (`sqlplus`) — port 1521**
```bash
docker run --rm -it gvenzl/oracle-free sqlplus scott/tiger@host.docker.internal:1521/FREE
```
```sql
SELECT * FROM emp WHERE deptno = 10;
```
(No real Oracle database is involved — OmniGate's `orawire` frontend speaks real Oracle Net
directly and translates to the bundled Postgres backend underneath. That's the whole point.)

On Linux, replace `host.docker.internal` with `172.17.0.1` (or run with `--network host`).
Already running natively rather than in Docker? Just use `localhost` instead.

## 3. Or skip the clients entirely

The **Get Started** tab in `/console` has a built-in query box — pre-filled with a working query
against the same demo data, one click to run it, no client needed at all.

## 4. Optional: turn on the local AI assistant

The console can also show a chat panel and offer to fix broken SQL you type, using a small model
that runs **entirely locally, CPU-only** (no data leaves the machine, no API key). It's optional
and not part of the default `docker compose up` — the free image doesn't bundle `llama-server` or
a model, so both need to be mounted into the container:

```bash
mkdir -p ~/.cache/omnigate-models
curl -L -o ~/.cache/omnigate-models/qwen2.5-1.5b-instruct-q4_k_m.gguf \
  "https://huggingface.co/Qwen/Qwen2.5-1.5B-Instruct-GGUF/resolve/main/qwen2.5-1.5b-instruct-q4_k_m.gguf"
```

Then add to the `omnigate` service in `docker-compose.yml`:
```yaml
    volumes:
      - ~/.cache/omnigate-models:/models:ro
      # a static, Linux-compiled llama-server binary — see llama.cpp's own releases
      - /path/to/llama-server:/usr/local/bin/llama-server:ro
    environment:
      OMNIGATE_ASSISTANT_LLAMA_SERVER_PATH: /usr/local/bin/llama-server
      OMNIGATE_ASSISTANT_MODEL_PATH: /models/qwen2.5-1.5b-instruct-q4_k_m.gguf
```

With that set, `/console` gets a chat panel (ask it things like "list my backends" or "test the
pg1 backend") and the query box gets an **"Ask AI to fix this"** button whenever a query fails.

## 5. What's next

- **[kumarrajamani-ai/Omnigate](https://github.com/kumarrajamani-ai/Omnigate)** — the full source
  repo, with `ARCHITECTURE.md` (the complete design doc, including exactly what's been
  live-verified vs. what's a known limit) and the commercial (unrestricted) edition.
- `/console`'s **Backends** tab lets you add a second backend (Snowflake, another Postgres,
  MySQL, Oracle, ...) live, no restart — either through the form or by asking the chat assistant.
  The free edition caps at 2 named backends; the commercial edition has no cap.
