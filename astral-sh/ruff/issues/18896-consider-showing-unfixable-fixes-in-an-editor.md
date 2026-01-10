```yaml
number: 18896
title: "Consider showing `unfixable` fixes in an editor context"
type: issue
state: open
author: ntBre
labels:
  - needs-design
  - diagnostics
assignees: []
created_at: 2025-06-23T14:48:16Z
updated_at: 2025-06-23T14:54:43Z
url: https://github.com/astral-sh/ruff/issues/18896
synced_at: 2026-01-10T11:09:59Z
```

# Consider showing `unfixable` fixes in an editor context

---

_Issue opened by @ntBre on 2025-06-23 14:48_

Currently, marking a rule as [unfixable](https://docs.astral.sh/ruff/settings/#lint_unfixable) will also suppress code actions in an editor. Do we want this to be the case, or should `unfixable` rules just be marked `DisplayOnly`? It looks like the LSP also filters out `DisplayOnly` fixes, so this might require additional changes there:

https://github.com/astral-sh/ruff/blob/b2b65e72eba8222f157fe3417ff7bf3ef034d122/crates/ruff_server/src/lint.rs#L247

See https://github.com/astral-sh/ruff/pull/18834#discussion_r2160074474 and the thread that follows for more details.
            

---

_Label `needs-design` added by @ntBre on 2025-06-23 14:48_

---

_Label `diagnostics` added by @ntBre on 2025-06-23 14:48_

---

_Comment by @MichaReiser on 2025-06-23 14:51_

This reads a bit strange :) I think it makes sense to add some prosa before quoting my review comment explaining what Ruff's current behavior is and what behavior we may want.

---
