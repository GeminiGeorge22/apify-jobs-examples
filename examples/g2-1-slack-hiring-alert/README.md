# G2-1 — Slack alert when a target account posts a sales or RevOps role

Proposed public path (when Master creates the repo): `examples/g2-1-slack-hiring-alert/` in `geminigeorge22/apify-jobs-examples`.

**Wall this fire:** `EXAMPLES_REPO_MISSING` — `https://github.com/GeminiGeorge22/apify-jobs-examples` returns 404 (Master H5 not landed). Local pack lives at `/workspace/g2-1-slack-hiring-alert/` until the repo exists.

## What this is

An n8n workflow that:

1. Runs `publicrecords/ashby-jobs-scraper` on a watchlist of Ashby board slugs
2. Filters titles/departments for sales / RevOps / GTM-ish roles
3. Posts a Slack alert to your hiring channel

## Files

| File | Purpose |
|---|---|
| `workflow.json` | n8n workflow export (import via UI) |
| `ashby-board-slugs.json` | Bundled ~200 slug watchlist + notes |
| `ashby-board-slugs.txt` | One slug per line |
| `article-draft.md` | Tutorial article (800–1500 words) |
| `run-sample.json` | Real publisher E2E sample (Apify + Slack ids) |
| `STATUS.md` | Honest done / walls for this fire |

## Import (n8n)

1. Open owner n8n → Workflows → Import from File → `workflow.json`
2. Create Header Auth credential: name `Apify Publisher Header Auth`, header `Authorization` = `Bearer <APIFY_TOKEN>` (publisher / publicrecords)
3. Create Slack credential with bot token that can `chat:write` to your channel
4. Edit **Buyer inputs**: companies / since / maxResults / slackChannel
5. Run once

## Cost (USD)

Pay-per-event on `publicrecords/ashby-jobs-scraper`:

- Run start: **$0.005**
- Job posting returned: **$0.0009** each

Demo bound used this fire: `maxResults=20`, `maxTotalChargeUsd=0.5` → theoretical max ≈ $0.005 + 20×$0.0009 = **$0.023**.

## E2E proof (publisher, 2026-10-01)

See `run-sample.json`. Summary:

- Apify run `2lLsvNkFl4kgHSpTQ` **SUCCEEDED**
- Dataset `vcNHUWY9a6wyjnugo` · items **20** · sales/RevOps matches **≥3**
- Slack `#publicrecords-monitors` (`C0C4G5A6TN0`) message ts recorded in `run-sample.json`

n8n execution id: **not available this fire** — Master self-hosted n8n not reachable on this box (`localhost:5678` closed). Workflow JSON is ready to import.
