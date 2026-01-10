```yaml
number: 21479
title: "[`flake8-simplify`] Fix truthiness assumption for non-iterable arguments in tuple/list/set calls (`SIM222`, `SIM223`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-21473
created_at: 2025-11-16T17:26:49Z
updated_at: 2025-12-01T22:37:05Z
url: https://github.com/astral-sh/ruff/pull/21479
synced_at: 2026-01-10T16:48:02Z
```

# [`flake8-simplify`] Fix truthiness assumption for non-iterable arguments in tuple/list/set calls (`SIM222`, `SIM223`)

---

_Pull request opened by @danparizher on 2025-11-16 17:26_

## Summary

Fixes false positives in SIM222 and SIM223 where truthiness was incorrectly assumed for `tuple(x)`, `list(x)`, `set(x)` when `x` is not iterable.

Fixes #21473.

## Problem

`Truthiness::from_expr` recursively called itself on arguments to iterable initializers (`tuple`, `list`, `set`) without checking if the argument is iterable, causing false positives for cases like `tuple(0) or True` and `tuple("") or True`.

## Approach

Added `is_definitely_not_iterable` helper and updated `Truthiness::from_expr` to return `Unknown` for non-iterable arguments (numbers, booleans, None) and string literals when called with iterable initializers, preventing incorrect truthiness assumptions.

## Test Plan

Added test cases to `SIM222.py` and `SIM223.py` for `tuple("")`, `tuple(0)`, `tuple(1)`, `tuple(False)`, and `tuple(None)` with `or True` and `and False` patterns.

---

_Comment by @astral-sh-bot[bot] on 2025-11-16 17:38_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Comment by @MichaReiser on 2025-11-16 18:26_

Thanks, @danparizher, for this PR. It's great to see how you jump on every Ruff issue. We appreciate your contributions greatly, and you're very productive. You're that productive that it is challenging for us to keep up with the number of PRs you submit; it can sometimes even feel a bit overwhelming to see 5 new PRs. 

That's why I'd like to ask you to give us some time to catch up. I think an ideal contribution experience for us (you as a contributor and we as a reviewer) would be to aim for a low one-digit number of open PRs that we aim to land before starting any new ones. That gives us the necessary time to properly review your PRs, as well as those from other contributors. 

I hope you won't receive this as discouraging because we really value your contributions and we would love to see more of them, maybe just have them a little more spaces out so that we can keep up :)









---

_Comment by @danparizher on 2025-11-16 18:34_

I'm not sure why this `ruff::cli analyze_graph` test is failing, I don't believe my changes are related to it.

Thanks so much for the kind words @MichaReiser! No worries at all, I completely get it—sorry for the PR flood! "Overzealous contributor" is probably a fitting title; I just get excited about Ruff 😄

I'll throttle back and stick to just a couple of open PRs at a time. I totally respect the need to keep the review pipeline manageable for you all and appreciate the heads-up!

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/helpers.rs`:1356 on 2025-11-17 21:32_

I think we can omit this branch. `Self::from_expr` should already work correctly for string literals. The problem in the issue is for t-strings, among other types, which behave differently.

Removing this also fixes the pytest snapshot, which I think was a true positive.

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/helpers.rs`:1347 on 2025-11-17 21:43_

I wonder if it might make more sense to list the types we explicitly want to recurse for instead of filtering out the cases where we don't want to recurse. Maybe just even a big `match` on `argument` would be easiest to reason about. What do you think?

Basically I think we need to return `Self::Unknown` here and avoid recursing for any type that has a definite `Self::Truthy` or `Self::Falsey` (or `Self::None`) type in one of the match arms above this, if the type might result in an empty iterable or will raise a type error.

As it stands, I don't think this PR actually fixes the t-string case from the issue.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM222.py`:222 on 2025-11-17 21:44_

We should add a t-string case:


```suggestion
tuple(t"") or True  # OK
```

---

_@ntBre requested changes on 2025-11-17 21:48_

---

_Label `bug` added by @ntBre on 2025-11-17 21:49_

---

_@danparizher reviewed on 2025-11-19 21:17_

---

_Review comment by @danparizher on `crates/ruff_python_ast/src/helpers.rs`:1347 on 2025-11-19 21:17_

That approach makes sense to me, thanks!

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/helpers.rs`:1350 on 2025-11-19 21:59_

I think we can recurse for all of these except t-strings. `StringLiteral`, `BytesLiteral`, and `FString` all check if they are empty or not. Only `TString` maps to `Self::Truthy` unconditionally.

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/helpers.rs`:1332 on 2025-11-19 22:00_

We can fold this `is_generator_expr` call into the match on `argument` below.

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/helpers.rs`:1387 on 2025-11-19 22:02_

I think it would be more clear if we grouped all of the expressions into one recursive arm and one `Self::Unknown` arm. Is the default `_` matching anything here? It seems like most cases are enumerated.

I'm kind of second-guessing my suggestion to list them all, but let's get the groups sorted out conclusively, and then we can decide to trim it down if it seems excessive.

---

_@ntBre reviewed on 2025-11-19 22:05_

---

_@danparizher reviewed on 2025-11-19 22:52_

---

_Review comment by @danparizher on `crates/ruff_python_ast/src/helpers.rs`:1387 on 2025-11-19 22:52_

The default `_` now matches the remaining `Expr` variants (collections like `List`, `Set`, `Tuple`, `Dict`; comprehensions; `Name`, `Call`, `Attribute`, etc.), which we recurse on.

---

_Review requested from @ntBre by @danparizher on 2025-11-20 00:56_

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/helpers.rs`:1340 on 2025-11-21 21:06_

We're still not recursing on string literals, and the pytest snapshot is still changing like I mentioned in https://github.com/astral-sh/ruff/pull/21479#discussion_r2543698051 and https://github.com/astral-sh/ruff/pull/21479#discussion_r2535574572.

---

_@ntBre requested changes on 2025-11-21 21:06_

---

_Review requested from @ntBre by @danparizher on 2025-11-29 00:36_

---

_@ntBre approved on 2025-12-01 21:57_

---

_Merged by @ntBre on 2025-12-01 21:57_

---

_Closed by @ntBre on 2025-12-01 21:57_

---

_Branch deleted on 2025-12-01 22:37_

---
