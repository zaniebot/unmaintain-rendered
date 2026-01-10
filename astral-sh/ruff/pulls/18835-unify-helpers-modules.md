```yaml
number: 18835
title: Unify helpers modules
type: pull_request
state: merged
author: dylwil3
labels:
  - internal
assignees: []
merged: true
base: main
head: unify-helpers
created_at: 2025-06-20T20:46:53Z
updated_at: 2025-06-20T21:03:12Z
url: https://github.com/astral-sh/ruff/pull/18835
synced_at: 2026-01-10T18:39:09Z
```

# Unify helpers modules

---

_Pull request opened by @dylwil3 on 2025-06-20 20:46_

A little bit of cleanup for consistency's sake: we move all the helpers modules to a consistent location, and update the import paths when needed. In the case of `refurb` there were two helpers modules, so we just merged them.

Happy to revert the last commit if people are okay with `super::super` I just thought it looked a little silly.

---

_Review requested from @ntBre by @dylwil3 on 2025-06-20 20:46_

---

_Label `internal` added by @dylwil3 on 2025-06-20 20:46_

---

_Comment by @github-actions[bot] on 2025-06-20 20:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-06-20 20:58_

Nice, thank you! I do prefer the `crate` imports over `super::super` too.

---

_Merged by @dylwil3 on 2025-06-20 21:03_

---

_Closed by @dylwil3 on 2025-06-20 21:03_

---

_Branch deleted on 2025-06-20 21:03_

---
