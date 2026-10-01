# Track competitors' hiring by department every week in Google Sheets

**Tutorial G2-3 · publicrecords/greenhouse-jobs-scraper · n8n · Google Sheets (competitive intel)**

Strategy and talent teams do not need a dump of every open role on the internet. They need a **weekly view of how named competitors are hiring by department** — Engineering vs Sales vs Product — already in Google Sheets so a Friday review takes minutes, not a browser marathon.

This tutorial wires that view with a public Apify Actor, an n8n workflow, and a Sheets tab you control. You will import a ready workflow, point it at a Greenhouse board watchlist, and see real department-labeled rows from a live publisher run.

## The problem

Competitive hiring intel usually fails in one of three ways:

1. LinkedIn alerts that mix every title into a single noisy stream
2. Manual career-page checks that nobody keeps up weekly
3. Broad ATS scrapes that leave you sorting Engineering from Sales by hand

Greenhouse boards are public JSON behind `boards.greenhouse.io/<token>` (and job-boards variants). The missing piece is a cheap, dated pull across a competitor list, a department label you can pivot on, and a destination your team already opens: **Google Sheets**.

## The result (real run)

On **2026-10-01** we ran `publicrecords/greenhouse-jobs-scraper` as the **publicrecords** publisher account through owner n8n with:

- `companies`: gitlab, cloudflare, datadog, twilio, discord, figma, robinhood, coinbase  
  (tutorial pack also ships ~200 Greenhouse board tokens; these eight are the proven E2E subset)
- `since`: `90d`
- `maxResults`: `60`

**Apify run id:** `H2UDWN6xDZ9YCUJ5k` — status **SUCCEEDED**  
**Dataset:** `RhccFGA1ZSAtCbFah` — **60** job postings returned  
**n8n:** workflow `xha9wufpsp9rlJoF` · execution `5` · status **success**  
**Google Sheets:** spreadsheet `16N1moNeErI0DUXJ9LUQ7Zxv83UWoDyLK5dOpw0ChXaU` · tab `CompetitorHiring` · rows **A2:J61** (60 new rows)

Department labels written this fire (title heuristics; Greenhouse returned null `department` fields for this window):

| departmentLabel | count |
|---|---|
| Engineering | 15 |
| Sales | 12 |
| Other | 10 |
| Operations | 6 |
| Product | 5 |
| People | 3 |
| Marketing | 3 |
| Finance | 2 |
| Data / Support / Design / Legal | 1 each |

Sample rows from the sheet:

- **Group Product Manager, Customer Engagement & Experience** at coinbase → Product — https://www.coinbase.com/careers/positions/8247881?gh_jid=8247881
- **Regional Manager, Sales Engineering - Majors (West)** at datadog → Sales — https://careers.datadoghq.com/detail/8231877/?gh_jid=8231877
- **Senior Software Engineer - Incident Insights & Readiness** at datadog → Engineering — https://careers.datadoghq.com/detail/8247566/?gh_jid=8247566
- **Senior Account Executive, SLED (DC Metro)** at cloudflare → Sales — https://boards.greenhouse.io/cloudflare/jobs/8241905?gh_jid=8241905
- **Staff Data Analyst** at gitlab → Data — https://job-boards.greenhouse.io/gitlab/jobs/8827370002

That is the artifact competitive intel wants: company, title, department label, location, deep link, and a scrape timestamp — not a unsorted career-page crawl.

## What you will build

1. An n8n workflow that schedules a weekly Greenhouse pull across a competitor watchlist
2. A Code node that assigns `departmentLabel` (Engineering / Sales / Product / Marketing / People / Finance / Legal / Operations / Support / Data / Design / Other)
3. A Google Sheets Append that writes review-ready columns
4. A ~200-token Greenhouse board list you can swap into Buyer inputs

## Cost per run (USD)

Pay-per-event on `publicrecords/greenhouse-jobs-scraper`:

| Item | Price |
|---|---|
| Run start (`actor-start`) | **$0.005** |
| Job posting returned (`job-posting`) | **$0.0009** each |

Demo bound used this fire: `maxResults=60`, `maxTotalChargeUsd=0.5`.

- Theoretical max ≈ $0.005 + 60 × $0.0009 = **$0.059**
- This E2E: 60 items → ≈ **$0.059**

Keep `maxTotalChargeUsd` set. Raise `maxResults` only after the department labels and sheet mapping look right.

## Import steps (n8n)

1. Open n8n → **Workflows** → **Import from File** → `workflow.json` from this folder (or from `examples/g2-3-competitors-hiring-sheets/` in `GeminiGeorge22/apify-jobs-examples`).
2. Create **Header Auth** credential: name `Apify Publisher Header Auth`, header `Authorization` = `Bearer <APIFY_TOKEN>` (use your own Apify token; publisher for demos).
3. Create **Google Sheets** OAuth credential and share the target spreadsheet with that Google account.
4. Edit **Buyer inputs**:
   - `actorInput` — JSON for `companies` / `since` / `maxResults` (start with the proven subset; swap in tokens from `greenhouse-board-slugs.json` when ready)
   - `sheetDocumentId` — your spreadsheet id
   - `sheetName` — tab name (create headers first: `company,title,department,departmentLabel,location,jobUrl,postedAt,ats,scrapedAt,competitorWatchlist`)
   - `maxTotalChargeUsd` — keep ≤ `0.5` for smoke runs
5. Run once (manual) and confirm:
   - Apify run **SUCCEEDED** with `itemCount ≥ 1`
   - Label node returned rows with `departmentLabel`
   - Sheets tab gained new rows
6. Switch the trigger to weekly when the smoke looks good.

## How department labeling works

Many Greenhouse boards leave the API `department` field empty even when the title is clear. The Code node therefore labels from the **title** (and any raw department string when present), with **Sales rules before Engineering** so titles like “Sales Engineer” land in Sales, not Engineering.

Buckets: Engineering, Sales, Product, Design, Marketing, People, Finance, Legal, Operations, Support, Data, Other.

You can tighten the regex list for your industry — the point is a stable column you can pivot in Sheets (`=QUERY` or a pivot table by `departmentLabel` × `company`).

## Extending the watchlist

The pack includes ~200 real startup Greenhouse board tokens in `greenhouse-board-slugs.json`. For production:

1. Keep the E2E eight until labels look right
2. Paste a larger `companies` array from the JSON (or maintain your own competitor CRM export)
3. Raise `maxResults` and `maxTotalChargeUsd` together
4. Optionally add a second Apify HTTP node for `publicrecords/lever-jobs-scraper` with the same label Code path if your competitors use Lever

## What this is not

- Not a claim that we scrape “all jobs” or run in real time
- Not affiliated with Greenhouse
- Not a gallery-ready n8n template until you clear your own `hold:submit` / partner review

We built the Actor (`publicrecords/greenhouse-jobs-scraper`). The tutorial sells the buyer’s job: a weekly competitor hiring sheet by department.

## Checklist

- [ ] Workflow imported; Apify Header Auth connected
- [ ] Sheets tab + headers created; OAuth connected
- [ ] Proven eight companies smoke run SUCCEEDED
- [ ] Pivot by `departmentLabel` looks sane
- [ ] Weekly schedule enabled
- [ ] `maxTotalChargeUsd` still set

## Links

- Actor: https://apify.com/publicrecords/greenhouse-jobs-scraper
- Examples folder: https://github.com/GeminiGeorge22/apify-jobs-examples/tree/main/examples/g2-3-competitors-hiring-sheets/
- Hub: https://geminigeorge22.github.io/greenhouse-jobs/
