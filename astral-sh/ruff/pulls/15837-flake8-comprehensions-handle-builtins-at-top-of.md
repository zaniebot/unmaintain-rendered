```yaml
number: 15837
title: "[`flake8-comprehensions`] Handle builtins at top of file correctly for `unnecessary-dict-comprehension-for-iterable` (`C420`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: dict-comp
created_at: 2025-01-30T21:30:56Z
updated_at: 2025-01-30T21:49:14Z
url: https://github.com/astral-sh/ruff/pull/15837
synced_at: 2026-01-12T15:55:52Z
```

# [`flake8-comprehensions`] Handle builtins at top of file correctly for `unnecessary-dict-comprehension-for-iterable` (`C420`)

---

_@dylwil3_

Builtin bindings are given a range of `0..0`, which causes strange behavior when range checks are made at the top of the file. In this case, the logic of the rule demands that the value of the dict comprehension is not self-referential (i.e. it does not contain definitions for any of the variables used within it). This logic was confused by builtins which looked like they were defined "in the comprehension", if the comprehension appeared at the top of the file.

Closes #15830

---

_Label `bug` added by @dylwil3 on 2025-01-30 21:30_

---

_Label `preview` added by @dylwil3 on 2025-01-30 21:30_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_dict_comprehension_for_iterable.rs`:106 on 2025-01-30 21:37_

Oh wow, great investigation! Could you more-or-less copy and paste your PR summary as a comment here? It's otherwise unclear why we have to special-case builtin bindings 😄

---

_@AlexWaygood approved on 2025-01-30 21:37_

Wow, what a bug! 😄

---

_Comment by @github-actions[bot] on 2025-01-30 21:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @dylwil3 on 2025-01-30 21:49_

---

_Closed by @dylwil3 on 2025-01-30 21:49_

---
