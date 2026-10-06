---
title: Authentication
description: Read when calling the Acme Orders API or debugging 401/403 responses.
---

# Authentication

Fictional example. Client-credentials flow; tokens are valid for 15 minutes.

```sh
curl -s -X POST https://auth.acme.invalid/token \
  -d grant_type=client_credentials -d scope=orders:read \
  -u "$ACME_CLIENT_ID:$ACME_CLIENT_SECRET"
```

Send the token as `Authorization: Bearer <token>`.

## Gotchas

- Credentials come from the environment (`ACME_CLIENT_ID`, `ACME_CLIENT_SECRET`); never ask the user to paste them.
- 401 after a long-running job: the token expired. Fetch a new one once and retry; fail on a second 401.
- 403 with `"error": "scope_missing"`: the client lacks a scope. Agents cannot grant scopes — tell the user which scope is missing.
- Pagination cursors are bound to the token that created them, see [pagination](pagination.md#gotchas).
