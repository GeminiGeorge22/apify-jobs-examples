# G2-1 status — 2026-10-01 S1 ~10:36 ET

**Status:** `in_progress`

## Exists

- n8n `workflow.json` (Apify → Code filter → Slack)
- `ashby-board-slugs.json` count=200 (proven E2E subset: ramp, linear + expanded demo boards)
- Article draft `article-draft.md`
- Publisher Apify run SUCCEEDED + Slack destination received artifact
- Local pack path `/workspace/g2-1-slack-hiring-alert/`

## Walls

| Wall | Platform wording / fact |
|---|---|
| `EXAMPLES_REPO_MISSING` | `GET https://github.com/GeminiGeorge22/apify-jobs-examples` → **404** (Master H5 not landed). Proposed path `examples/g2-1-slack-hiring-alert/`. |
| `N8N_HOST_UNAVAILABLE` | `curl http://127.0.0.1:5678` → connection refused. Master owns self-hosted n8n; no owner URL/creds in Marketer secret store for remote import. tried: localhost:5678. |

## Not done yet

- n8n execution id on owner account
- Publish to examples repo + n8n template gallery + dev.to + Hashnode (blocked on examples repo / n8n host)
