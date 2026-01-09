---
number: 20784
title: clear_diagnostics sends a publishDiagnostics notification without checking the client capabilities
type: issue
state: closed
author: pyscripter
labels:
  - help wanted
  - server
assignees: []
created_at: 2025-10-09T10:54:26Z
updated_at: 2025-10-28T18:24:36Z
url: https://github.com/astral-sh/ruff/issues/20784
synced_at: 2026-01-07T13:12:16-06:00
---

# clear_diagnostics sends a publishDiagnostics notification without checking the client capabilities

---

_Issue opened by @pyscripter on 2025-10-09 10:54_

### Summary

Whilst [publish_diagnostics](https://github.com/astral-sh/ruff/blob/f0d0b5790079cc38ec0f4b34d771a056f1cd1481/crates/ty_server/src/server/api/diagnostics.rs#L142C15-L142C34) checks client capabilities for supports_pull_diagnostics before sending a notification,
[clear_diagnostics](https://github.com/astral-sh/ruff/blob/f0d0b5790079cc38ec0f4b34d771a056f1cd1481/crates/ty_server/src/server/api/diagnostics.rs#L129) sends a publishDiagnostics notification without doing so.

Although it does not do much harm, I think it should do the same check as publish_diagnostics

### Version

0.14

---

_Label `help wanted` added by @MichaReiser on 2025-10-09 12:19_

---

_Label `server` added by @MichaReiser on 2025-10-09 12:19_

---

_Referenced in [astral-sh/ruff#20989](../../astral-sh/ruff/pulls/20989.md) on 2025-10-20 11:08_

---

_Closed by @MichaReiser on 2025-10-20 17:31_

---

_Comment by @pyscripter on 2025-10-20 18:15_

@MichaReiser  The fix is for ty.  Please apply the same fix to the ruff server.  https://github.com/astral-sh/ruff/blob/f0d0b5790079cc38ec0f4b34d771a056f1cd1481/crates/ruff_server/src/server/api/diagnostics.rs

---

_Reopened by @MichaReiser on 2025-10-20 19:00_

---

_Comment by @TaKO8Ki on 2025-10-23 08:41_

I will open a follow-up pull request for that.

---

_Referenced in [astral-sh/ruff#21105](../../astral-sh/ruff/pulls/21105.md) on 2025-10-28 05:40_

---

_Closed by @MichaReiser on 2025-10-28 18:24_

---

_Referenced in [astral-sh/ruff#21188](../../astral-sh/ruff/pulls/21188.md) on 2025-11-01 19:19_

---
