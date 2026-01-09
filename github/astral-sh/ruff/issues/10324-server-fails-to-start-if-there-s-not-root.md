---
number: 10324
title: "Server fails to start if there's not root directory provided"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - server
assignees: []
created_at: 2024-03-11T05:08:01Z
updated_at: 2024-03-18T15:46:45Z
url: https://github.com/astral-sh/ruff/issues/10324
synced_at: 2026-01-07T13:12:15-06:00
---

# Server fails to start if there's not root directory provided

---

_Issue opened by @dhruvmanila on 2024-03-11 05:08_

(Posting from Discord for tracking purposes.)

The server fails to start if there's no root directory provided in the initialization parameters:

```
ruff failed
  Cause: No workspace or root URI was given in the LSP initialization parameters. The server cannot start.
```

We default to the current working directory if that's not provided in `ruff-lsp`: https://github.com/astral-sh/ruff-lsp/blob/c410d6b44c1b40a41d254d13e33535bd198935e0/ruff_lsp/server.py#L1656-L1664

/cc @snowsignal 

---

_Label `bug` added by @dhruvmanila on 2024-03-11 05:08_

---

_Label `server` added by @dhruvmanila on 2024-03-11 05:09_

---

_Assigned to @snowsignal by @snowsignal on 2024-03-12 17:36_

---

_Referenced in [astral-sh/ruff#10398](../../astral-sh/ruff/pulls/10398.md) on 2024-03-14 00:15_

---

_Closed by @snowsignal on 2024-03-18 15:46_

---
