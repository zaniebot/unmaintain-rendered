```yaml
number: 14020
title: "[red-knot] have mdformat wrap mdtest files to 100 columns"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/mdformat
created_at: 2024-10-31T15:10:18Z
updated_at: 2024-10-31T21:09:53Z
url: https://github.com/astral-sh/ruff/pull/14020
synced_at: 2026-01-12T15:55:46Z
```

# [red-knot] have mdformat wrap mdtest files to 100 columns

---

_@carljm_

This makes it easier to read and edit (and review changes to) these files as source, even though it doesn't affect the rendering.

---

_Label `red-knot` added by @carljm on 2024-10-31 15:10_

---

_Review requested from @MichaReiser by @carljm on 2024-10-31 15:10_

---

_Review requested from @AlexWaygood by @carljm on 2024-10-31 15:10_

---

_@AlexWaygood approved on 2024-10-31 15:13_

Thank you!! I was hoping there was a tool that would do this automatically for us

---

_Comment by @github-actions[bot] on 2024-10-31 15:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-10-31 15:39_

Not sure about the limit to be 80, considering that blackendocs use 130. Also, what tiny screen do you all have :P

---

_Comment by @AlexWaygood on 2024-10-31 16:07_

I'd be okay with increasing the limit here to 100 and reducing the blacken-docs limit to 100, if we want a consistent wrapping.

> Also, what tiny screen do you all have :P

Hehe. How do I do split-screening on my 14" laptop in a coffee shop with lines as long as you write them 😆

---

_Renamed from "[red-knot] have mdformat wrap mdtest files to 80 columns" to "[red-knot] have mdformat wrap mdtest files to 100 columns" by @carljm on 2024-10-31 17:14_

---

_@MichaReiser approved on 2024-10-31 17:26_

LGTM. Consider waiting till after @sharkdp's PR for unbound landed to not create more (and very annoying to that) merge conflicts for him.

What's funny about this PR is that it makes most lines longer haha

---

_Comment by @sharkdp on 2024-10-31 19:07_

> LGTM. Consider waiting till after @sharkdp's PR for unbound landed to not create more (and very annoying to that) merge conflicts for him.

Apparently, there are no conflicts. But thanks for waiting!

---

_Review requested from @sharkdp by @carljm on 2024-10-31 20:56_

---

_Merged by @carljm on 2024-10-31 21:00_

---

_Closed by @carljm on 2024-10-31 21:00_

---

_Branch deleted on 2024-10-31 21:00_

---
