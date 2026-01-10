```yaml
number: 16191
title: "[`flake8-debugger`] Also flag `sys.breakpointhook` and `sys.__breakpointhook__` (`T100`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: main
head: brent/t100-breakpointhook
created_at: 2025-02-16T18:18:22Z
updated_at: 2025-02-16T19:50:18Z
url: https://github.com/astral-sh/ruff/pull/16191
synced_at: 2026-01-10T19:57:22Z
```

# [`flake8-debugger`] Also flag `sys.breakpointhook` and `sys.__breakpointhook__` (`T100`)

---

_Pull request opened by @ntBre on 2025-02-16 18:18_

## Summary

Fixes #16189.

Only `sys.breakpointhook` is flagged by the upstream linter:
https://github.com/pylint-dev/pylint/blob/007a745c8619c2cbf59f829a8f09fc6afa6eb0f1/pylint/checkers/stdlib.py#L38

but I think it makes sense to flag [`__breakpointhook__`](https://docs.python.org/3/library/sys.html#sys.__breakpointhook__) too, as suggested in the issue because it 
> contain[s] the original value of breakpointhook [...] in case [it happens] to get replaced with broken or alternative objects.

## Test Plan

New T100 test cases


---

_Label `rule` added by @ntBre on 2025-02-16 18:18_

---

_Comment by @github-actions[bot] on 2025-02-16 18:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2025-02-16 18:28_

---

_Merged by @ntBre on 2025-02-16 19:50_

---

_Closed by @ntBre on 2025-02-16 19:50_

---

_Branch deleted on 2025-02-16 19:50_

---
