# G2-1 status — 2026-10-01 S1 ~13:16 ET

**Status:** `file` (draft→file this fire)

## Exists / proven

- n8n workflow `7D9jp4mnOeRqtSnr` on owner host (publicrecords@agentmail.to)
- n8n execution `3` status=success
- Apify run `bMCg5dTUaxhWzohgt` SUCCEEDED dataset=`7Nkgc0dsjGmgdpvYf` items=20 matched_sales_revops=6
- Slack `C0C4G5A6TN0` ts_first=`1790874974.269719` (6 messages)
- Examples repo: https://github.com/GeminiGeorge22/apify-jobs-examples/tree/main/examples/g2-1-slack-hiring-alert/
- Pack: workflow.json (Code node fixed: `const all = items.flatMap...`) + 200 slugs + article-draft.md

## Walls cleared

| Wall | Clear |
|---|---|
| `N8N_HOST_UNAVAILABLE` | Owner n8n restarted on 127.0.0.1:5678 with prior n8n-data; import+run OK |
| `EXAMPLES_REPO_MISSING` | Created public `GeminiGeorge22/apify-jobs-examples`; landed examples/g2-1-slack-hiring-alert/ |

## Hold

`hold:submit` — do **not** submit to n8n template gallery / partner apps.
