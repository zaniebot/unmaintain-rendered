```yaml
number: 22750
title: "Add new option `format.markdown-code-format`"
type: pull_request
state: open
author: amyreese
labels:
  - do-not-merge
assignees: []
draft: true
base: main
head: amy/markdown-code-format-option
created_at: 2026-01-20T00:45:56Z
updated_at: 2026-01-21T00:37:25Z
url: https://github.com/astral-sh/ruff/pull/22750
synced_at: 2026-01-21T00:50:46Z
```

# Add new option `format.markdown-code-format`

---

_@amyreese_

Adds a new formatting option to enable formatting markdown code blocks.


Issue #22636


---

_Comment by @astral-sh-bot[bot] on 2026-01-20 00:53_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Comment by @MichaReiser on 2026-01-20 06:57_

I don't know if you planned to do so or if this is just a prototype PR but I think this may require some design decision, because this new option is orthogonal to ruff's existing include/exclude system but sort of does the same

---

_Label `do-not-merge` added by @amyreese on 2026-01-21 00:37_

---
