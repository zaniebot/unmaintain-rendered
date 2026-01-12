```yaml
number: 2457
title: Ability to disable all type checks, to keep only the LSP server
type: issue
state: closed
author: ilan-schemoul
labels: []
assignees: []
created_at: 2026-01-12T10:27:28Z
updated_at: 2026-01-12T10:45:23Z
url: https://github.com/astral-sh/ty/issues/2457
synced_at: 2026-01-12T11:01:03Z
```

# Ability to disable all type checks, to keep only the LSP server

---

_Issue opened by @ilan-schemoul on 2026-01-12 10:27_

Hello,
The pylsp server is used is horribly slow (like 1 minute for rename or go to definition). I want to use ty for LSP features, and when it hits feature parity with mypy (we have very complex type system at my company) I'll enable ty for type check.

So as long as I have mypy, I want an option to disable type checking in editor (LSP) settings.

---

_Comment by @MichaReiser on 2026-01-12 10:45_

You can disable all diagnostics by setting [`diagnosticMode`](https://docs.astral.sh/ty/reference/editor-settings/#diagnosticmode) to `off`

---

_Closed by @MichaReiser on 2026-01-12 10:45_

---
