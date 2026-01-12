```yaml
number: 9698
title: "RUF022, RUF023: never add two trailing commas to the end of a sequence"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-syntax-errors2
created_at: 2024-01-30T09:43:31Z
updated_at: 2024-01-30T17:19:41Z
url: https://github.com/astral-sh/ruff/pull/9698
synced_at: 2026-01-12T15:55:30Z
```

# RUF022, RUF023: never add two trailing commas to the end of a sequence

---

_@AlexWaygood_

Fixes the issues highlighted in https://github.com/astral-sh/ruff/issues/8402#issuecomment-1916203707 and https://github.com/astral-sh/ruff/issues/8402#issuecomment-1916213693

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-01-30 09:43_

---

_Comment by @github-actions[bot] on 2024-01-30 09:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-01-30 10:15_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF022_RUF022.py.snap`:1022 on 2024-01-30 10:15_

It's hard to say where a comment like `# strange comment 1` should go really, if it lies between the string item and the associated comma. Possibly it should move with `"foo"`, even though it's directly above `"bar"`. But this is a very unlikely edge case, and the important thing is that the comment isn't deleted, in my opinion.

---

_Label `bug` added by @AlexWaygood on 2024-01-30 12:10_

---

_Label `autofix` added by @AlexWaygood on 2024-01-30 12:10_

---

_@charliermarsh reviewed on 2024-01-30 16:49_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:447 on 2024-01-30 16:49_

Do you mind adding a comment here to explain the logic? I think I understand now: we need the formatted value to end in a trailing comma, but if it's part of the postlude, it'll be inserted verbatim anyway.

---

_@charliermarsh approved on 2024-01-30 16:49_

---

_@AlexWaygood reviewed on 2024-01-30 16:50_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:447 on 2024-01-30 16:50_

yeah, exactly. I'll add a comment!

---

_@MichaReiser approved on 2024-01-30 17:07_

---

_Merged by @AlexWaygood on 2024-01-30 17:19_

---

_Closed by @AlexWaygood on 2024-01-30 17:19_

---

_Branch deleted on 2024-01-30 17:19_

---
