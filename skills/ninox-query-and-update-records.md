---
name: Query and update records in a table
description: Discover a workspace's modules and tables, read records with pagination, and update them.
api: openapi/ninox-public-openapi-original.json
operations:
  - ModulesV1Controller_getModules
  - TablesV1Controller_getTables
  - RowsV1Controller_getRows
  - RowsV1Controller_updateRows
---

# Query and update records in a table

Navigate the Ninox workspace hierarchy, read records, and patch them.

## Auth
`Authorization: Bearer {apiKey}` (workspace-scoped API key). Base URL: `https://go.ninox.com`.

## Steps
1. **List modules** — `ModulesV1Controller_getModules`
   `GET /api/v1/workspace/{workspaceId}/modules`
   Pick the target module name.
2. **List tables** — `TablesV1Controller_getTables`
   `GET /api/v1/workspace/{workspaceId}/modules/{moduleName}/tables`
   Pick the target table name.
3. **Read records** — `RowsV1Controller_getRows`
   `GET /api/v1/workspace/{workspaceId}/modules/{moduleName}/tables/{tableName}/records`
   Page with `limit`/`offset`; each record has an `id` and a `values` map.
4. **Update records** — `RowsV1Controller_updateRows`
   `PATCH /api/v1/workspace/{workspaceId}/modules/{moduleName}/tables/{tableName}/records`
   Send each record's `id` plus the changed `values`.

## Conventions & errors
- Keep paging while `page_info.has_more` is true.
- Updates are not idempotent-keyed; send only changed fields and confirm with a follow-up read.
- Error envelope `{ "error": { "message": string } }`; handle 400/401/404/500.
