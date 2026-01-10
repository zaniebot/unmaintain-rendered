```yaml
number: 17824
title: "[`pylint`] add fix safety section (`PLE4703`)"
type: pull_request
state: merged
author: yunchipang
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/fix-safety-modified-iterating-set
created_at: 2025-05-03T20:23:56Z
updated_at: 2025-05-12T21:40:14Z
url: https://github.com/astral-sh/ruff/pull/17824
synced_at: 2026-01-10T18:51:01Z
```

# [`pylint`] add fix safety section (`PLE4703`)

---

_Pull request opened by @yunchipang on 2025-05-03 20:23_

This PR adds a fix safety section in comment for rule PLE4703.

parent: #15584 
impl was introduced at #970 (couldn't find newer PRs sorry!)

---

_Label `documentation` added by @AlexWaygood on 2025-05-03 20:37_

---

_Comment by @github-actions[bot] on 2025-05-03 20:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre reviewed on 2025-05-09 20:21_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/modified_iterating_set.rs`:31 on 2025-05-09 20:21_

This sounds mostly similar to the description of the rule above. I think it's enough to say it's always unsafe because it changes the program's behavior (by making code that would previously raise an error actually run).

---

_Review requested from @ntBre by @yunchipang on 2025-05-09 21:48_

---

_@ntBre approved on 2025-05-12 20:27_

---

_Merged by @ntBre on 2025-05-12 20:27_

---

_Closed by @ntBre on 2025-05-12 20:27_

---

_Branch deleted on 2025-05-12 21:40_

---
