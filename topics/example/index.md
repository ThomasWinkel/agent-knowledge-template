---
title: Example Topic (fictional)
description: Fictional Acme Orders API that demonstrates the topic format. Removed by manage.py init.
---

> **Agents:** read [the knowledge base guide](../../AGENTS.md) first.

# Example Topic (fictional)

The Acme Orders API is a **fictional** internal REST API, used only to demonstrate how a topic is structured. A real entry point states in 2-5 sentences what the topic covers and when an agent needs it.

## Key facts

- Base URL `https://orders.acme.invalid/api/v2`. v1 is shut down; ignore v1 examples found elsewhere.
- Every request needs a bearer token: [authentication](authentication.md).
- List endpoints use cursors, never offsets: [pagination](pagination.md).

<!-- BEGIN GENERATED CONTENTS: do not edit, run `python .knowledge-base/manage.py index` -->
## Contents

- [Authentication](authentication.md) — Read when calling the Acme Orders API or debugging 401/403 responses.
- [Pagination](pagination.md) — Read when listing orders or other collections from the Acme Orders API.
<!-- END GENERATED CONTENTS -->
