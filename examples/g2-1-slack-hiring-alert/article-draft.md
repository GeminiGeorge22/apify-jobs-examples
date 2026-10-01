# Get a Slack alert when a target account posts a sales or RevOps role

**Tutorial G2-1 · publicrecords/ashby-jobs-scraper · n8n**

Hiring for sales and RevOps is a timing game. The companies you care about post roles on Ashby (and other ATS boards) all week; by the time someone forwards the careers page, the best candidates are already in process. You do not need another spreadsheet. You need a narrow alert: *this watched account just posted a sales or RevOps role*.

This tutorial wires that up with a public Apify Actor and an n8n workflow that posts into Slack. You will import a ready workflow, point it at a board watchlist, and see a real alert built from a live publisher run.

## The problem

Talent and GTM teams usually do one of three things:

1. Bookmark a handful of career pages and check them manually
2. Follow LinkedIn hoping the algorithm surfaces the right roles
3. Pay for a broad job board scrape that floods Slack with noise

None of those is "tell me when Ramp or Notion opens an AE / RevOps / partnerships seat." Ashby boards are public JSON behind `jobs.ashbyhq.com/<board>`; the missing piece is a cheap, dated pull plus a title filter and a destination your team already reads.

## The result (real run)

On **2026-10-01** we ran `publicrecords/ashby-jobs-scraper` as the **publicrecords** publisher account with:

- `companies`: ramp, linear, notion, vercel, retool, mercury, plaid, brex, rippling, gusto, deel, lattice, gong, apollo, attio, posthog, sentry, launchdarkly, webflow, framer
- `since`: `30d`
- `maxResults`: `20`

**Apify run id:** `2lLsvNkFl4kgHSpTQ` — status **SUCCEEDED**  
**Dataset:** `vcNHUWY9a6wyjnugo` — **20** job postings  
**Slack:** channel `C0C4G5A6TN0` · message ts `1790865703.223109`

Sales / RevOps matches from that batch:

- **Forward Deployed Architect** at notion (Sales) — https://jobs.ashbyhq.com/notion/8cb74a17-1685-4f92-8314-6ef6264b5085
- **Partner Development Representative | Accounting** at ramp (Sales) — https://jobs.ashbyhq.com/ramp/b55447c0-4adc-42eb-9ca2-f88fd44e0e5b
- **Global Head of Land Revenue** at notion (Sales) — https://jobs.ashbyhq.com/notion/33ca1c55-e2e4-49a2-a09f-68c83cb540e3
- **Technical Support Specialist** at attio (GTM) — https://jobs.ashbyhq.com/attio/285f98e6-0b65-4688-b381-8ab6b7cff1ca

That is the artifact you want in Slack: role title, company, department, and a deep link to the Ashby posting — not a dump of every engineering requisition.

## What you will build

1. An n8n schedule (daily is enough for most watches)
2. An HTTP call to Apify `run-sync-get-dataset-items` for `publicrecords/ashby-jobs-scraper`
3. A small Code node that keeps sales / RevOps / GTM-ish titles
4. A Slack message into `#hiring` or your monitors channel

A bundled watchlist of **200** Ashby board slugs ships beside the workflow (`ashby-board-slugs.json`). The demo run above used a cheaper 20-board subset so the charge stayed under a few cents.

## Cost per run (USD)

The Actor is pay-per-event:

| Event | Price |
|---|---|
| Run start | $0.005 |
| Job posting returned | $0.0009 |

With `maxResults=20` the ceiling is about **$0.023** per run ($0.005 + 20 × $0.0009), before you widen the watchlist. Keep `maxTotalChargeUsd` at **0.5** (or lower) on the Apify call while you are testing.

## Import steps

### 1. Prerequisites

- Apify account with access to run `publicrecords/ashby-jobs-scraper` (Store rental / publisher token)
- n8n (Cloud or self-hosted) where you can import JSON
- Slack app / bot with `chat:write` on the destination channel

### 2. Import the workflow

1. Download `workflow.json` from this folder (or from `examples/g2-1-slack-hiring-alert/` once the public examples repo exists)
2. In n8n: **Workflows → Import from File**
3. Open the sticky note on the canvas for the short setup checklist

### 3. Credentials

1. **Apify Header Auth** — HTTP Header Auth credential, header name `Authorization`, value `Bearer <your Apify token>`
2. **Slack** — bot token credential used by the Slack node (or swap the Slack node for an HTTP Request to `chat.postMessage` if you prefer)

Do not paste tokens into the workflow JSON. Credentials stay in n8n.

### 4. Buyer inputs

Edit the **Buyer inputs** Set node:

- `actorInput` — JSON string with `companies`, `since`, `maxResults`
- `maxTotalChargeUsd` — e.g. `0.5`
- `slackChannel` — channel id (the template defaults to a monitors channel id; change it)

To watch the full bundled list, paste slugs from `ashby-board-slugs.txt` into `companies` (or load the file in a prior node). Start with 10–20 boards until you like the noise level.

### 5. Filter logic

The **Filter sales / RevOps** Code node keeps postings whose title or department matches patterns such as sales, account executive, RevOps, BDR/SDR, customer success, GTM, and partnerships. Tune the regex list for your ICP — for example, drop customer success if you only want quota-carrying AEs.

### 6. Run once

Execute the workflow manually. Confirm:

1. Apify run **SUCCEEDED** in Console
2. Dataset item count ≥ 1 (or honest zero if nothing matched `since`)
3. Slack message appeared with title + link

For dated windows, `since=30d` is a good first smoke; switch to `24h` for the daily alert once the boards are warm.

## Expand the watchlist

`ashby-board-slugs.json` holds ~200 curated board URL slugs (`jobs.ashbyhq.com/<slug>`). Not every slug is guaranteed live forever — boards move — so treat the file as a starting watchlist. The **proven** subset used in kit tasks and this tutorial's E2E is at least `ramp` and `linear`.

If you also track Greenhouse or Lever, clone the workflow and point the Apify URL at `publicrecords/greenhouse-jobs-scraper` or `publicrecords/lever-jobs-scraper` with the same filter node.

## Publishing note

The intended public home for this pack is `geminigeorge22/apify-jobs-examples` → `examples/g2-1-slack-hiring-alert/`. As of 2026-10-01 that GitHub repository still returns **404**, so this pack is distributed from the Marketer workspace / fleet-exchange until Master lands the examples repo. Community template submission to n8n's gallery stays on hold with the same wall.

## Why this beats a spreadsheet

You get a dated pull (only postings since your window), a role filter your GTM team cares about, and a destination that already creates interrupt-driven follow-up. The Actor bills per posting returned, so quiet days stay cheap. Swap the Slack node for email or a CRM webhook when you outgrow the channel.

---

*Voice: clear tutorial. Actor: publicrecords/ashby-jobs-scraper. Sample run and Slack ts are real publisher artifacts from 2026-10-01.*
