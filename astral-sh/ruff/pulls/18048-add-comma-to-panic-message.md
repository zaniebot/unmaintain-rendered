```yaml
number: 18048
title: Add comma to panic message
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/comma
created_at: 2025-05-12T15:47:25Z
updated_at: 2025-05-12T16:26:56Z
url: https://github.com/astral-sh/ruff/pull/18048
synced_at: 2026-01-10T18:51:01Z
```

# Add comma to panic message

---

_Pull request opened by @charliermarsh on 2025-05-12 15:47_

## Summary

Consistent with other variants of this, separate the conditional clause.


---

_Review requested from @carljm by @charliermarsh on 2025-05-12 15:47_

---

_Review requested from @MichaReiser by @charliermarsh on 2025-05-12 15:47_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-05-12 15:47_

---

_Review requested from @sharkdp by @charliermarsh on 2025-05-12 15:47_

---

_Review requested from @dcreager by @charliermarsh on 2025-05-12 15:47_

---

_@AlexWaygood approved on 2025-05-12 15:50_

---

_Comment by @github-actions[bot] on 2025-05-12 15:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @charliermarsh on 2025-05-12 15:52_

---

_Closed by @charliermarsh on 2025-05-12 15:52_

---

_Branch deleted on 2025-05-12 15:52_

---

_Label `ty` added by @charliermarsh on 2025-05-12 15:53_

---

_Comment by @MichaReiser on 2025-05-12 16:21_

I think my terminal incorrectly rendered the URL, including the `,`. Arguably, that's a terminal bug, but it's the reason why I omitted the comma.

---

_Comment by @charliermarsh on 2025-05-12 16:24_

Can we render it on its own line, like we do for Ruff? Or does the diagnostic system not accommodate that?

---

_Comment by @MichaReiser on 2025-05-12 16:26_

It might look odd. Let me test this again. Worst case is that someone opens a PR with `[panic],`

---
