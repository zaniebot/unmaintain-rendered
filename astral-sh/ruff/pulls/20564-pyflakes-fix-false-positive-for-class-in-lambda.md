```yaml
number: 20564
title: "[`pyflakes`] Fix false positive for `__class__` in lambda expressions within class definitions (`F821`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-20562
created_at: 2025-09-25T03:38:01Z
updated_at: 2025-10-24T14:27:03Z
url: https://github.com/astral-sh/ruff/pull/20564
synced_at: 2026-01-10T17:28:45Z
```

# [`pyflakes`] Fix false positive for `__class__` in lambda expressions within class definitions (`F821`)

---

_Pull request opened by @danparizher on 2025-09-25 03:38_

## Summary

Fixes #20562


---

_Comment by @github-actions[bot] on 2025-09-25 03:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:2984 on 2025-09-25 20:17_

I was picturing this in `visit_stmt` like we did for function definitions rather than in the deferred visit. I guess lambdas aren't statements, so the analogous code would actually fit here in `visit_expr`:

https://github.com/astral-sh/ruff/blob/e4ac9e90419841a338d6909af01acc8548c8b3c3/crates/ruff_linter/src/checkers/ast/mod.rs#L1620-L1632

I think that should work because we push a `lambda` snapshot that gets used here like we do for functions. The one downside is that the popping for lambdas happens a bit farther down, unlike functions:

https://github.com/astral-sh/ruff/blob/e4ac9e90419841a338d6909af01acc8548c8b3c3/crates/ruff_linter/src/checkers/ast/mod.rs#L2062-L2063

---

_@ntBre reviewed on 2025-09-25 20:27_

Thank you! The tests look good, but what do you think about moving the new code to `visit_expr` to mirror the function version a bit more closely?

---

_Comment by @danparizher on 2025-09-26 01:40_

Yeah, that approach makes sense to me. Thanks for the review!

---

_Review requested from @ntBre by @danparizher on 2025-09-26 01:54_

---

_Label `bug` added by @ntBre on 2025-10-21 20:31_

---

_Comment by @ntBre on 2025-10-21 21:20_

Hmm, it looks like there are some new false positives in the ecosystem report. I was hoping it was something simple with the scope popping, which is why I pushed a commit, but that didn't end up helping. We're flagging a case like this with F401:

```py
import uuid


class C:
    uuid = lambda: str(uuid.uuid4())
```

and I don't fully understand why. We'll need to figure it out before we can merge this, though. Would you mind taking a look?

---

_Comment by @danparizher on 2025-10-24 04:59_

The issue was that the `DunderClassCell` scope was being added during lambda definition processing instead of during lambda body analysis, which caused false positives when lambdas within classes referenced module-level variables like `uuid`.

I fixed it by moving the scope logic from `visit_expr` to `visit_deferred_lambdas` (similar to how function definitions work) and changing the scope detection to check the entire scope hierarchy rather than just the current scope, ensuring `__class__` is accessible in lambdas within classes while preventing false positives for other variables.

---

_@ntBre approved on 2025-10-24 14:03_

---

_Comment by @ntBre on 2025-10-24 14:03_

Ah okay, I guess my earlier suggestion about moving it to `visit_expr` was wrong then. Thank you for debugging and sorry for the bad suggestion!

---

_Merged by @ntBre on 2025-10-24 14:07_

---

_Closed by @ntBre on 2025-10-24 14:07_

---

_Branch deleted on 2025-10-24 14:27_

---
