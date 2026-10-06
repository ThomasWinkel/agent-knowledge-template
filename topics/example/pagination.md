---
title: Pagination
description: Read when listing orders or other collections from the Acme Orders API.
---

# Pagination

Fictional example. List endpoints return at most `limit` items (default 50, max 200) plus `next_cursor`.

```sh
curl -s -H "Authorization: Bearer $TOKEN" \
  "https://orders.acme.invalid/api/v2/orders?limit=200&cursor=$CURSOR"
```

Repeat with `cursor=<next_cursor>` until `next_cursor` is `null`.

## Gotchas

- `offset` is accepted but silently ignored — always the first page.
- Cursors expire with the token that created them ([authentication](authentication.md)). For long exports, refresh the token only between full runs or restart from the first page.
