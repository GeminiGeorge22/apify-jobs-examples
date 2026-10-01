# G2-3 — Track competitors' hiring by department every week in Google Sheets

**Examples:** [`GeminiGeorge22/apify-jobs-examples`](https://github.com/GeminiGeorge22/apify-jobs-examples/tree/main/examples/g2-3-competitors-hiring-sheets/) · path `examples/g2-3-competitors-hiring-sheets/`.

**Filed 2026-10-01 S1:** n8n workflow `xha9wufpsp9rlJoF` execution `5` + Apify `H2UDWN6xDZ9YCUJ5k` + Sheets `16N1moNeErI0DUXJ9LUQ7Zxv83UWoDyLK5dOpw0ChXaU` tab `CompetitorHiring` rows `A2:J61`. See `verify.md`.

## What this is

An n8n workflow that:

1. Runs `publicrecords/greenhouse-jobs-scraper` on a competitor Greenhouse board watchlist
2. Labels each job with a department bucket (Engineering / Sales / Product / Marketing / …)
3. Appends weekly rows to Google Sheets for competitive intel

## Files

| File | Purpose |
|---|---|
| `workflow.json` | n8n workflow export (import via UI; Google Sheets + Apify Header Auth) |
| `greenhouse-board-slugs.json` | Bundled ~200 board tokens + proven E2E subset |
| `greenhouse-board-slugs.txt` | One token per line |
| `article-draft.md` | Tutorial article (800–1500 words) |
| `run-sample.json` | Real publisher E2E sample (Apify + n8n + Sheets ids) |
| `STATUS.md` | Honest done / holds for this fire |
| `verify.md` | Findings + platform IDs |

## Import (n8n)

1. Workflows → Import from File → `workflow.json`
2. Header Auth: `Authorization: Bearer <APIFY_TOKEN>`
3. Google Sheets OAuth → set `sheetDocumentId` / `sheetName` in **Buyer inputs**
4. Create header row on the tab before first append (`company,title,department,departmentLabel,location,jobUrl,postedAt,ats,scrapedAt,competitorWatchlist`)
5. Run once

## Cost (USD)

- Run start: **$0.005**
- Job posting: **$0.0009** each
- Demo: `maxResults=60`, `maxTotalChargeUsd=0.5` → max ≈ **$0.059**

## E2E proof (publisher, 2026-10-01)

See `run-sample.json`. Summary:

- Apify `H2UDWN6xDZ9YCUJ5k` **SUCCEEDED** · dataset `RhccFGA1ZSAtCbFah` · items **60**
- n8n execution **`5`** (owner host)
- Sheets spreadsheet `16N1moNeErI0DUXJ9LUQ7Zxv83UWoDyLK5dOpw0ChXaU` · `CompetitorHiring!A2:J61`

`hold:submit` — do not submit to n8n template gallery / partner apps.
