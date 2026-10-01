# verify.md — growth/g2-2-salesforce-admins-sheets

## Findings

- **Stage:** none → draft → **file** (2026-10-01 S1 ~13:43 ET)
- Owner n8n self-host on box reused `/workspace/s1-2026-09-30/n8n-data` (encryptionKey s1-20260930); owner `publicrecords@agentmail.to`.
- Created workflow `MigmodStRUNleVwu`, ran via `n8n execute --id=MigmodStRUNleVwu` with companies `[vibe, companycam, medely, jump-app]` since=`180d` maxResults=`40`.
- Filter returned **4** Salesforce Administrator rows; Apify run `IKM1vdCITqBEyxXSe` SUCCEEDED.
- Google Sheets destination (Larry/Sheets): spreadsheet `16N1moNeErI0DUXJ9LUQ7Zxv83UWoDyLK5dOpw0ChXaU` tab `SalesforceAdmins` updatedRange `SalesforceAdmins!A2:H5` (4 rows).
- Public examples path prepared under `examples/g2-2-salesforce-admins-sheets/` (hold:submit — no gallery submit).

## Platform IDs

| Field | Value |
|---|---|
| n8n_workflow_id | `MigmodStRUNleVwu` |
| n8n_execution_id | `4` |
| apify_run_id | `IKM1vdCITqBEyxXSe` |
| dataset_id | `kmGcLcyHPZ2cER0In` |
| itemCount | `39` |
| matched_salesforce_admin | `4` |
| apify_status | `SUCCEEDED` |
| spreadsheetId | `16N1moNeErI0DUXJ9LUQ7Zxv83UWoDyLK5dOpw0ChXaU` |
| sheetName | `SalesforceAdmins` |
| sheets_row_range | `SalesforceAdmins!A2:H5` |
| sheets_rows | `4` |
| examples_repo | https://github.com/GeminiGeorge22/apify-jobs-examples |
| examples_path | `examples/g2-2-salesforce-admins-sheets/` |

## Tried

- 2026-10-01T13:34:00-04:00 by=S1 — G1 re-verify HTTP 200×4; G2-2 pack + E2E; draft→file
