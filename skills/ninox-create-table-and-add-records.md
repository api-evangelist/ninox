---
name: Create a table and add records
description: Provision a new Ninox table with fields inside an existing module, then add and read back records.
api: openapi/ninox-public-openapi-original.json
operations:
  - TablesV1Controller_createTable
  - FieldsV1Controller_createFieldsBatch
  - RowsV1Controller_addRows
  - RowsV1Controller_getRows
---

# Create a table and add records

Use the Ninox Public API to stand up a table in an existing module and load data into it.

## Auth
All requests send the workspace API key as a bearer token:
`Authorization: Bearer {apiKey}`
Generate keys in Workspace Integration settings. The key is scoped to a single workspace.
Base URL: `https://go.ninox.com`.

## Steps
1. **Create the table** — `TablesV1Controller_createTable`
   `POST /api/v1/workspace/{workspaceId}/modules/{moduleName}/tables`
   Provide the new table name in the body. Capture the returned table identifier/name.
2. **Add fields (batch)** — `FieldsV1Controller_createFieldsBatch`
   `POST /api/v1/workspace/{workspaceId}/modules/{moduleName}/tables/{tableName}/fields/batch`
   Define the typed columns in one call.
3. **Add records** — `RowsV1Controller_addRows`
   `POST /api/v1/workspace/{workspaceId}/modules/{moduleName}/tables/{tableName}/records`
   Send a `values` map keyed by field name for each new record.
4. **Read records back** — `RowsV1Controller_getRows`
   `GET /api/v1/workspace/{workspaceId}/modules/{moduleName}/tables/{tableName}/records`
   Paginate with `limit`/`offset`; keep paging while `page_info.has_more` is true.

## Conventions & errors
- Pagination is offset-based (`limit`, `offset`) with `page_info { has_more, limit, offset }`.
- No idempotency key is supported; do not blindly retry POSTs — check state first.
- Errors return `{ "error": { "message": string } }`. Handle 400 (bad body), 401 (bad/missing key), 404 (missing workspace/module/table), 413 (payload too large).
