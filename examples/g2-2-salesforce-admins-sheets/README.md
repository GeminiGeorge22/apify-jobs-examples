# G2-2 — Weekly Salesforce-admin hiring list in Google Sheets

**Examples:** [`GeminiGeorge22/apify-jobs-examples`](https://github.com/GeminiGeorge22/apify-jobs-examples/tree/main/examples/g2-2-salesforce-admins-sheets/) · path `examples/g2-2-salesforce-admins-sheets/`.

**Filed 2026-10-01 S1:** n8n workflow `MigmodStRUNleVwu` execution `4` + Apify `IKM1vdCITqBEyxXSe` + Sheets `16N1moNeErI0DUXJ9LUQ7Zxv83UWoDyLK5dOpw0ChXaU` tab `SalesforceAdmins` rows `A2:H5`. See `verify.md`.

## What this is

An n8n workflow that:

1. Runs `publicrecords/ashby-jobs-scraper` on an Ashby board watchlist
2. Filters titles/departments for Salesforce Administrator / admin / RevOps-adjacent Salesforce roles
3. Appends Clay-ready rows to Google Sheets

## Files

| File | Purpose |
|---|---|
| `workflow.json` | n8n workflow export (import via UI; Google Sheets + Apify Header Auth) |
| `ashby-board-slugs.json` | Bundled ~200 slug watchlist + proven E2E subset |
| `ashby-board-slugs.txt` | One slug per line |
| `article-draft.md` | Tutorial article (800–1500 words) |
| `run-sample.json` | Real publisher E2E sample (Apify + n8n + Sheets ids) |
| `STATUS.md` | Honest done / holds for this fire |
| `verify.md` | Findings + platform IDs |

## Import (n8n)

1. Workflows → Import from File → `workflow.json`
2. Header Auth: `Authorization: Bearer <APIFY_TOKEN>`
3. Google Sheets OAuth → set `sheetDocumentId` / `sheetName` in **Buyer inputs**
4. Create header row on the tab before first append
5. Run once

## Cost (USD)

- Run start: **$0.005**
- Job posting: **$0.0009** each
- Demo: `maxResults=40`, `maxTotalChargeUsd=0.5` → max ≈ **$0.041**

## E2E proof (publisher, 2026-10-01)

See `run-sample.json`. Summary:

- Apify `IKM1vdCITqBEyxXSe` **SUCCEEDED** · dataset `kmGcLcyHPZ2cER0In` · items **39** · matches **4**
- n8n execution **`4`** (owner host)
- Sheets spreadsheet `16N1moNeErI0DUXJ9LUQ7Zxv83UWoDyLK5dOpw0ChXaU` · `SalesforceAdmins!A2:H5`

`hold:submit` — do not submit to n8n template gallery / partner apps.
