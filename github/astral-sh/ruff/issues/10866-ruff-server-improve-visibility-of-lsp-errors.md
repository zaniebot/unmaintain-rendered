---
number: 10866
title: "`ruff server`: Improve visibility of LSP errors"
type: issue
state: closed
author: snowsignal
labels:
  - server
assignees: []
created_at: 2024-04-10T23:43:24Z
updated_at: 2024-04-16T18:32:55Z
url: https://github.com/astral-sh/ruff/issues/10866
synced_at: 2026-01-07T13:12:15-06:00
---

# `ruff server`: Improve visibility of LSP errors

---

_Issue opened by @snowsignal on 2024-04-10 23:43_

As part of improving `ruff server`'s logging capabilities, we should implement a mechanism to send [`window/showMessage`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#window_showMessage) notifications to the client. 

This will enable us to report critical issues to the user in a more visible way:
<img width="461" alt="Screenshot 2024-04-10 at 4 39 44 PM" src="https://github.com/astral-sh/ruff/assets/19577865/4d513e0d-7f32-4519-a422-d6e0901afc23">

---

_Label `server` added by @snowsignal on 2024-04-10 23:43_

---

_Added to milestone `Ruff Server: Beta` by @snowsignal on 2024-04-10 23:43_

---

_Assigned to @snowsignal by @snowsignal on 2024-04-10 23:43_

---

_Referenced in [astral-sh/ruff#10951](../../astral-sh/ruff/pulls/10951.md) on 2024-04-15 10:10_

---

_Closed by @snowsignal on 2024-04-16 18:32_

---
