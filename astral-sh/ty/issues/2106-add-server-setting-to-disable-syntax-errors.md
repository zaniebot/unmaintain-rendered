```yaml
number: 2106
title: Add server setting to disable syntax errors
type: issue
state: closed
author: MichaReiser
labels:
  - help wanted
  - server
assignees: []
created_at: 2025-12-19T10:06:58Z
updated_at: 2025-12-27T20:24:40Z
url: https://github.com/astral-sh/ty/issues/2106
synced_at: 2026-01-12T15:54:26Z
```

# Add server setting to disable syntax errors

---

_@MichaReiser_

Similar to ruff's [`showSyntaxErrors`](https://docs.astral.sh/ruff/editors/settings/#showsyntaxerrors) option. An option like this is useful if you want to use ty alongside other Python LSPs (like Ruff) to avoid seeing multiple diagnostics for the same syntax error.

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-19 10:06_

---

_Label `server` added by @MichaReiser on 2025-12-19 10:07_

---

_Comment by @MichaReiser on 2025-12-19 10:07_

The most "trivial" implementation here would be to filter out syntax errors on the LSP side. 

---

_Label `help wanted` added by @MichaReiser on 2025-12-19 10:07_

---

_Closed by @MichaReiser on 2025-12-27 20:24_

---
