# G2-2 status — 2026-10-01 S1 ~13:43 ET

**Status:** `file` (none→draft→file this fire)

## Exists / proven

- n8n workflow `MigmodStRUNleVwu` on owner host (publicrecords@agentmail.to)
- n8n execution `4` status=success (CLI execute against owner n8n-data)
- Apify run `IKM1vdCITqBEyxXSe` SUCCEEDED dataset=`kmGcLcyHPZ2cER0In` items=39 matched_salesforce_admin=4
- Google Sheets spreadsheetId=`16N1moNeErI0DUXJ9LUQ7Zxv83UWoDyLK5dOpw0ChXaU` tab=`SalesforceAdmins` rows=`A2:H5` (Larry/Sheets publisher connection)
- Examples repo path: `examples/g2-2-salesforce-admins-sheets/`
- Pack: workflow.json + 200 slugs + article-draft.md + run-sample.json

## Notes

- Owner E2E used Manual trigger + Apify + filter on n8n; Sheets rows appended via Larry/Sheets (publisher) because this host has no Google Sheets OAuth credential in n8n. Public `workflow.json` includes the Google Sheets Append node for buyer import.
- Proven companies: vibe, companycam, medely, jump-app (live Salesforce Administrator postings).

## Hold

`hold:submit` — do **not** submit to n8n template gallery / partner apps.
