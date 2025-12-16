# n8n SOC Alert Enrichment (SIEM → Enrich → Score → Ticket/Notify)

A practical SecOps automation demo that takes a security alert (from a SIEM/webhook), enriches indicators (IP/domain/hash),
applies a simple risk score, and routes the result to a ticketing/notification destination.

> **Why this project exists:** Show recruiters you can build repeatable, production-minded security workflows (retries, error paths, rate limits, secrets).

## What it does
- **Input:** Webhook payload (example JSON included) containing indicators like `src_ip`, `domain`, `sha256`.
- **Enrichment:** Calls threat intel APIs (examples wired for AbuseIPDB + VirusTotal + WhoisXML placeholders).
- **Scoring:** Adds a basic score (0–100) based on API responses.
- **Output:** Demonstrates 2 routes:
  1) **High risk** → create ticket (stub) + notify (Slack/Telegram/email stub)
  2) **Low/medium risk** → log/store (Google Sheets stub)

## Screenshots / Demo
Add:
- A screenshot of the workflow canvas
- A screenshot of one execution showing enrich + score
- A short GIF (optional)

## Architecture
```
SIEM / Source → n8n Webhook → Normalize → Enrich (APIs) → Score → Route → Ticket / Notify / Store
```

## Quick start (local)
1. Install Docker + Docker Compose
2. Copy env file:
   ```bash
   cp .env.example .env
   ```
3. Start n8n:
   ```bash
   docker compose up -d
   ```
4. Import workflow:
   - Open n8n: http://localhost:5678
   - Import: `workflows/workflow.json`
5. Trigger sample:
   ```bash
   curl -X POST http://localhost:5678/webhook/alert-enrich \
     -H "Content-Type: application/json" \
     -d @samples/alert.json
   ```

## Environment variables
Set these in `.env`:
- `ABUSEIPDB_API_KEY`
- `VIRUSTOTAL_API_KEY`
- `WHOISXML_API_KEY` (optional)

## Security notes
- Never hardcode API keys in workflows.
- Use n8n credentials + environment variables.
- Add rate limiting + caching for production.

## Roadmap ideas (to make it even stronger)
- Add a cache (Redis) to avoid repeat lookups
- Add a “suppression / allowlist” table
- Add a “case summary” markdown generator for tickets
- Add unit tests for the scoring logic (as a JS node module)

---
**Created:** 2025-12-16
