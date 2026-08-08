# OmniGate — Free Edition

**Connect your existing Oracle, Postgres, or MySQL tools to any database — no driver changes, no app rewrites.**

OmniGate sits in front of your database and speaks Oracle, Postgres, and MySQL all at once. Point whatever client, ORM, or BI tool you already use at OmniGate instead of your database directly, and it just works — even if your actual database is a completely different kind.

<img src="docs/architecture.svg" alt="Oracle, Postgres, MySQL, native OmniGate, and MCP agent clients all connect through a load balancer to a cluster of OmniGate nodes, which pool out to Oracle, Postgres, MySQL, Snowflake, Redshift, BigQuery, any REST API, and S3-compatible object storage (CSV/XML/Parquet) — with SQL firewall, NL2SQL, cross-source joins, sharding, replication, failover, caching, and observability applying across every request.">

Five different ways in — Oracle clients, Postgres clients, MySQL clients, apps using OmniGate's
own native driver, and AI agents over MCP — all land on the same cluster and can reach any of
your connected data sources, including ones that aren't traditional databases at all: REST APIs
and files sitting in S3-compatible object storage. Nobody has to change their tools.

## Why

- **Use the client you already have.** Oracle scripts, Postgres ORMs, MySQL BI dashboards, or an AI agent talking MCP — all keep working unchanged.
- **Any data source, one gateway.** Oracle, Postgres, MySQL, Snowflake, Redshift, BigQuery, any REST API, and CSV/XML/Parquet files in S3-compatible object storage — connect what you have, add more later, live, no restart.
- **Join across all of them, in one query.** Combine rows from a database with a CSV file in object storage, or with a REST API, in a single SQL statement — not a separate export/import step.
- **Ask it questions in plain English.** NL2SQL turns a typed-out question into SQL and runs it — with a confirmation step before anything that changes data.
- **Faster on the second ask.** Frequently-run queries are cached automatically, so repeat requests don't hit the original source again.
- **A built-in web console.** See what's connected, run a query, and — with the optional local AI assistant — get help fixing a broken query, right in your browser.
- **Nothing to install for your team.** OmniGate is the only new thing; everyone's existing tools connect exactly as they normally would.
- **Built to grow with you.** Run one instance for a small setup, or a load-balanced cluster with automatic failover for production — same product either way.

## Get started in under a minute

```bash
git clone https://github.com/kumarrajamani-ai/omnigate-free.git
cd omnigate-free
docker compose up
```

Then open **http://localhost:8080/console** — it walks you through trying it with your own client, or a query box right in the browser. Full walkthrough: **[SKILLS.md](SKILLS.md)**.

## This is the free edition

Free to use, no signup, no time limit — capped at 100 concurrent connections and 2 connected
databases. Need more? The commercial edition removes both caps — **contact the repo owner
([@kumarrajamani-ai](https://github.com/kumarrajamani-ai)) to get access.**
