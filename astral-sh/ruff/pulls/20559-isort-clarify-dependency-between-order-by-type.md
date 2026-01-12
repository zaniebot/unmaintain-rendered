```yaml
number: 20559
title: "[`isort`] Clarify dependency between `order-by-type` and `case-sensitive` settings"
type: pull_request
state: merged
author: ntBre
labels:
  - documentation
assignees: []
merged: true
base: main
head: brent/update-docs-case-sensitive
created_at: 2025-09-24T19:48:36Z
updated_at: 2025-09-25T16:30:29Z
url: https://github.com/astral-sh/ruff/pull/20559
synced_at: 2026-01-12T15:57:04Z
```

# [`isort`] Clarify dependency between `order-by-type` and `case-sensitive` settings

---

_@ntBre_

Summary
--

Fixes #20536 by linking between the isort options `case-sensitive` and `order-by-type`. The latter takes precedence over the former, so it seems good to clarify this somewhere.

I tweaked the wording slightly, but this is otherwise based on the patch from @SkylerWittman in https://github.com/astral-sh/ruff/issues/20536#issuecomment-3326097324 (thank you!)

Test Plan
--

N/a


---

_Label `documentation` added by @ntBre on 2025-09-24 19:48_

---

_Comment by @github-actions[bot] on 2025-09-24 19:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-09-24 19:57_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:2306 on 2025-09-25 16:12_

Nit: Maybe option to make it clearer what this refers to?
```suggestion
    /// Note that this option takes precedence over the
```

---

_@MichaReiser approved on 2025-09-25 16:12_

---

_Merged by @ntBre on 2025-09-25 16:25_

---

_Closed by @ntBre on 2025-09-25 16:25_

---

_Branch deleted on 2025-09-25 16:25_

---
