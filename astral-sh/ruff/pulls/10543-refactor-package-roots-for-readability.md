```yaml
number: 10543
title: "Refactor `package_roots` for readability."
type: pull_request
state: merged
author: dizzy57
labels:
  - internal
assignees: []
merged: true
base: main
head: refactor-package-roots
created_at: 2024-03-24T09:05:12Z
updated_at: 2024-03-24T14:35:24Z
url: https://github.com/astral-sh/ruff/pull/10543
synced_at: 2026-01-10T22:47:02Z
```

# Refactor `package_roots` for readability.

---

_Pull request opened by @dizzy57 on 2024-03-24 09:05_

## Summary
Replaced explicit `match` with a call to `hash_map::Entry::or_insert_with` to reduce code nesting.

## Test Plan
`cargo test`.

---

_Comment by @github-actions[bot] on 2024-03-24 09:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-03-24 12:02_

Nice

---

_Label `internal` added by @MichaReiser on 2024-03-24 12:02_

---

_Merged by @MichaReiser on 2024-03-24 12:02_

---

_Closed by @MichaReiser on 2024-03-24 12:02_

---

_Branch deleted on 2024-03-24 14:35_

---
