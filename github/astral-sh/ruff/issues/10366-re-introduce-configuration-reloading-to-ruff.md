---
number: 10366
title: "Re-introduce configuration reloading to `ruff server`"
type: issue
state: closed
author: snowsignal
labels:
  - configuration
  - server
assignees: []
created_at: 2024-03-12T17:34:08Z
updated_at: 2024-03-21T20:17:08Z
url: https://github.com/astral-sh/ruff/issues/10366
synced_at: 2026-01-07T13:12:15-06:00
---

# Re-introduce configuration reloading to `ruff server`

---

_Issue opened by @snowsignal on 2024-03-12 17:34_

## Summary

File-based configuration reload was removed from https://github.com/astral-sh/ruff/pull/10158 because it was using a file-watcher on the server side, which is actually not recommended by the LSP specification. Instead, we should listen to the `didChangeWatchedFiles` notification to reload configuration.

---

_Label `server` added by @snowsignal on 2024-03-12 17:34_

---

_Assigned to @snowsignal by @snowsignal on 2024-03-12 17:34_

---

_Label `configuration` added by @snowsignal on 2024-03-12 17:37_

---

_Referenced in [astral-sh/ruff#10399](../../astral-sh/ruff/pulls/10399.md) on 2024-03-14 00:57_

---

_Referenced in [astral-sh/ruff#10404](../../astral-sh/ruff/pulls/10404.md) on 2024-03-14 02:47_

---

_Closed by @snowsignal on 2024-03-21 20:17_

---
