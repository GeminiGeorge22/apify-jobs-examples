# Build a weekly list of companies hiring Salesforce admins in Google Sheets

**Tutorial G2-2 · publicrecords/ashby-jobs-scraper · n8n · Google Sheets (Clay-ready)**

Outbound and RevOps teams do not need another generic “tech jobs” feed. They need a short, weekly list of companies that just opened a **Salesforce Administrator** (or RevOps-adjacent Salesforce) seat — already in Google Sheets so Clay, enrichment, and sequencing can pick it up without a copy-paste ritual.

This tutorial wires that list with a public Apify Actor, an n8n workflow, and a Sheets tab you control. You will import a ready workflow, point it at an Ashby board watchlist, and see real Salesforce-admin rows from a live publisher run.

## The problem

Most “hiring signal” stacks fail Salesforce-admin outbound in one of three ways:

1. LinkedIn alerts that bury admin roles under AE noise
2. Broad ATS scrapes that dump every engineering requisition into a sheet
3. Manual career-page checks that nobody keeps up weekly

Ashby boards are public JSON behind `jobs.ashbyhq.com/<board>`. The missing piece is a cheap, dated pull, a title filter tuned for Salesforce admin / administrator / GTM-systems Salesforce roles, and a destination Clay already reads: **Google Sheets**.

## The result (real run)

On **2026-10-01** we ran `publicrecords/ashby-jobs-scraper` as the **publicrecords** publisher account through owner n8n with:

- `companies`: vibe, companycam, medely, jump-app  
  (tutorial pack also ships ~200 Ashby board slugs; these four are the proven E2E subset that had live Salesforce Administrator postings)
- `since`: `180d`
- `maxResults`: `40`

**Apify run id:** `IKM1vdCITqBEyxXSe` — status **SUCCEEDED**  
**Dataset:** `kmGcLcyHPZ2cER0In` — **39** job postings scanned  
**n8n:** workflow `MigmodStRUNleVwu` · execution `4` · status **success**  
**Google Sheets:** spreadsheet `16N1moNeErI0DUXJ9LUQ7Zxv83UWoDyLK5dOpw0ChXaU` · tab `SalesforceAdmins` · rows **A2:H5** (4 new rows)

Salesforce-admin matches written to the sheet:

- **Salesforce Administrator** at companycam (Sales) — https://jobs.ashbyhq.com/companycam/b2e4c9a0-84fa-45f7-8a18-e88c81a38277
- **Senior Salesforce Administrator** at vibe (Revenue) — https://jobs.ashbyhq.com/vibe/f3e7c6b0-1cef-49c7-a0b9-a967171b666d
- **Senior Salesforce Administrator (GTM Systems)** at medely (Sales Department) — https://jobs.ashbyhq.com/medely/e2d1c4a7-37b3-4219-8741-609c7e70f5cc
- **Global Salesforce Administrator** at jump-app — https://jobs.ashbyhq.com/jump-app/5c473f46-62d8-4603-bf59-eeeafe2f4785

That is the artifact you want before Clay: company, title, department, location, deep link, and a scrape timestamp — not a dump of every backend role on the same boards.

## What you will build

1. An n8n workflow that schedules a weekly Ashby pull
2. A Code node that keeps Salesforce admin / administrator / RevOps-adjacent Salesforce titles
3. A Google Sheets Append that writes Clay-ready columns
4. A ~200-slug Ashby watchlist you can swap into Buyer inputs

## Cost per run (USD)

Pay-per-event on `publicrecords/ashby-jobs-scraper`:

| Event | Price |
|---|---|
| Run start (`actor-start`) | **$0.005** |
| Job posting returned (`job-posting`) | **$0.0009** each |

Demo bound used this fire: `maxResults=40`, `maxTotalChargeUsd=0.5`.

- Theoretical max ≈ $0.005 + 40 × $0.0009 = **$0.041**
- This E2E: 39 items → ≈ **$0.040** (publisher usage read-back ~$0.00037 compute; PPE dominates)

Keep `maxTotalChargeUsd` set. Raise `maxResults` only after the filter and sheet mapping look right.

## Import steps (n8n)

1. Open n8n → **Workflows** → **Import from File** → `workflow.json` from this folder (or from `examples/g2-2-salesforce-admins-sheets/` in `GeminiGeorge22/apify-jobs-examples`).
2. Create **Header Auth** credential: name `Apify Publisher Header Auth`, header `Authorization` = `Bearer <APIFY_TOKEN>` (use your own Apify token; publisher for demos).
3. Create **Google Sheets** OAuth credential and share the target spreadsheet with that Google account.
4. Edit **Buyer inputs**:
   - `actorInput` — JSON for `companies` / `since` / `maxResults` (start with the proven subset; swap in slugs from `ashby-board-slugs.json` when ready)
   - `sheetDocumentId` — your spreadsheet id
   - `sheetName` — tab name (create headers first: `company,title,department,location,jobUrl,postedAt,matchedKeyword,scrapedAt`)
   - `maxTotalChargeUsd` — keep ≤ `0.5` for smoke runs
5. Run once (manual) and confirm:
   - Apify run **SUCCEEDED** with `itemCount ≥ 1`
   - Filter node returned matched rows (or an explicit `matched: 0` note)
   - Sheets tab gained new rows
6. Switch the trigger to weekly when the smoke looks good.

## Sheet columns (Clay-ready)

| Column | Use |
|---|---|
| company | Clay company key / domain join |
| title | Role filter / personalization |
| department | Segment (Sales / Revenue / GTM) |
| location | Geo routing |
| jobUrl | Proof + sequence footnote |
| postedAt | Freshness |
| matchedKeyword | Which regex fired |
| scrapedAt | Run audit |

Clay can treat each row as a company+role signal: enrich domain, find the hiring manager, and sequence only when `title` matches your ICP.

## Expanding the watchlist

`ashby-board-slugs.json` ships **200** board slugs: the four proven Salesforce-admin boards plus the broader startup list from G2-1. For weekly production:

- Put the full slug array into `actorInput.companies` (or batch in chunks of ~40–50 if you want smaller sheets per run)
- Keep `since` at `7d` or `30d` once the backfill is done
- Leave the Code filter as-is unless you need stricter “Administrator only” (drop the broad `/salesforce/i` last pattern)

## What this is not

- Not a LinkedIn scrape
- Not a request to paste your Apify token into a public workflow field (use Header Auth)
- Not an n8n template-gallery submission (this pack stays `hold:submit`)

## Verify checklist

- [ ] Workflow JSON imports without credential ids from our host
- [ ] Apify run id **SUCCEEDED**, dataset `itemCount ≥ 1`
- [ ] Filter returns ≥1 Salesforce-admin row on the proven subset (or honest `matched: 0` on a cold week)
- [ ] Google Sheets tab shows the new rows (spreadsheet id + range)
- [ ] n8n execution id recorded on your host

## Source

- Actor: https://apify.com/publicrecords/ashby-jobs-scraper  
- Examples: https://github.com/GeminiGeorge22/apify-jobs-examples/tree/main/examples/g2-2-salesforce-admins-sheets/  
- Related: G2-1 Slack hiring alert (sales/RevOps) — same Actor, different destination and filter
