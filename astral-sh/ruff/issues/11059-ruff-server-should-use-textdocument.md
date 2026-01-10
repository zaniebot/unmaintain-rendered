```yaml
number: 11059
title: "`ruff server` should use `textDocument/publishDiagnostics` when pull diagnostics aren't supported"
type: issue
state: closed
author: snowsignal
labels:
  - server
assignees: []
created_at: 2024-04-20T22:20:07Z
updated_at: 2024-04-23T04:06:37Z
url: https://github.com/astral-sh/ruff/issues/11059
synced_at: 2026-01-10T11:09:53Z
```

# `ruff server` should use `textDocument/publishDiagnostics` when pull diagnostics aren't supported

---

_Issue opened by @snowsignal on 2024-04-20 22:20_

Right now, `ruff server` only uses pull diagnostics to send diagnostics to the client - they aren't pro-actively sent. This means that any LSP client that doesn't send a pull diagnostics request [will not have visible diagnostics](https://github.com/astral-sh/ruff/issues/11022). This includes Neovim, Helix, and potentially others at the time of this writing, though Neovim will support pull diagnostics in the upcoming `0.10.0` release.

We should still support pull diagnostics going forward, but if they aren't supported (which the server will check using the client capabilities passed in) we should fall back to a publish diagnostics mode as an alternative.


---

_Label `server` added by @snowsignal on 2024-04-20 22:20_

---

_Added to milestone `Ruff Server: Beta` by @snowsignal on 2024-04-20 22:20_

---

_Assigned to @snowsignal by @snowsignal on 2024-04-20 22:20_

---

_Closed by @snowsignal on 2024-04-23 04:06_

---
