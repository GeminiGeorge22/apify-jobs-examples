# verify.md — growth/g2-1-slack-hiring-alert

## Findings

- **Stage:** draft → **file** (2026-10-01 S1 ~13:16 ET)
- Owner n8n self-host on box reused `/workspace/s1-2026-09-30/n8n-data` (encryptionKey s1-20260930); owner `publicrecords@agentmail.to`.
- Imported workflow, wired Apify Header Auth (`Bl7oCf4HGpvvpCdB`) + Slack bot (`1lvDHRIepLjDAMCP`), fixed Code node TDZ (`const items = items...` → `const all = items...`), ran manually with companies `[ramp, linear]` since=`30d` maxResults=`20`.
- Public examples repo created and pack landed under `examples/g2-1-slack-hiring-alert/` (hold:submit — no gallery submit).
- Prior publisher-only E2E retained: apify_run=`2lLsvNkFl4kgHSpTQ` dataset=`vcNHUWY9a6wyjnugo` items=20 matched=4 slack_ts=`1790865703.223109`.

## Platform IDs

| Field | Value |
|---|---|
| n8n_workflow_id | `7D9jp4mnOeRqtSnr` |
| n8n_execution_id | `3` |
| apify_run_id | `bMCg5dTUaxhWzohgt` |
| dataset_id | `7Nkgc0dsjGmgdpvYf` |
| itemCount | `20` |
| matched_sales_revops | `6` |
| apify_status | `SUCCEEDED` |
| slack_channel | `C0C4G5A6TN0` |
| slack_ts_first | `1790874974.269719` |
| slack_message_count | `6` |
| examples_repo | https://github.com/GeminiGeorge22/apify-jobs-examples |
| examples_path | `examples/g2-1-slack-hiring-alert/` |
| examples_commit | `2ba73bc` (Code node fix) / initial `fa86ff1` |

## Tried

- 2026-10-01T10:42:00-04:00 by=S1 — publisher Apify+Slack without n8n host / examples repo (walls open)
- 2026-10-01T13:16:00-04:00 by=S1 — n8n host + examples repo + execution 3 success (file)
