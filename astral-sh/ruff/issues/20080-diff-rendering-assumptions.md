```yaml
number: 20080
title: Diff rendering assumptions
type: issue
state: open
author: ntBre
labels:
  - diagnostics
assignees: []
created_at: 2025-08-25T13:16:28Z
updated_at: 2025-09-15T09:15:54Z
url: https://github.com/astral-sh/ruff/issues/20080
synced_at: 2026-01-12T15:54:57Z
```

# Diff rendering assumptions

---

_@ntBre_

> I only just realized that our fix infrastructure always assumes that all changes belong to the file from the primary span. 
>
> How would the rendered diagnostic look like if we had a sub-diagnostic with a span pointing to a different file? I don't think it's something we have to solve now but it might be worth documenting if the rendered output looks confusing.

_Originally posted by @MichaReiser in https://github.com/astral-sh/ruff/pull/20036#discussion_r2296000447_

Beyond the implicit fix-primary-span connection, we'll also render diffs immediately after the last diagnostic, whether that fits nicely or not. Especially after the planned changes in #19919, the diffs will be expecting a `help:` header above them, which will be missing if an additional sub-diagnostic has been added before the `help` message.

These issues shouldn't affect any existing diagnostics yet, but they're something to look out for as we start using the new diagnostic features more extensively.
            

---

_Label `diagnostics` added by @ntBre on 2025-08-25 13:19_

---

_Renamed from "Diagnostic rendering assumptions" to "Diff rendering assumptions" by @MichaReiser on 2025-09-15 09:15_

---
