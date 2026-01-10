```yaml
number: 15703
title: "[`ruff`] Parenthesize fix when argument spans multiple lines for `unnecessary-round` (`RUF057`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - fixes
  - preview
assignees: []
merged: true
base: main
head: round-fix
created_at: 2025-01-24T00:11:07Z
updated_at: 2025-01-24T10:34:57Z
url: https://github.com/astral-sh/ruff/pull/15703
synced_at: 2026-01-10T19:57:22Z
```

# [`ruff`] Parenthesize fix when argument spans multiple lines for `unnecessary-round` (`RUF057`)

---

_Pull request opened by @dylwil3 on 2025-01-24 00:11_

As in the title. We also make the fix unsafe when the range of the call to `round()` intersects comments.

Closes #15598 


---

_Label `bug` added by @dylwil3 on 2025-01-24 00:11_

---

_Label `fixes` added by @dylwil3 on 2025-01-24 00:11_

---

_Label `preview` added by @dylwil3 on 2025-01-24 00:11_

---

_Comment by @github-actions[bot] on 2025-01-24 00:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-01-24 09:59_

---

_Merged by @dylwil3 on 2025-01-24 10:34_

---

_Closed by @dylwil3 on 2025-01-24 10:34_

---
