```yaml
number: 14203
title: "FBT001: exclude boolean operators"
type: pull_request
state: merged
author: randolf-scholz
labels:
  - rule
assignees: []
merged: true
base: main
head: fbt_boolean_operators
created_at: 2024-11-08T14:10:42Z
updated_at: 2024-11-10T22:49:45Z
url: https://github.com/astral-sh/ruff/pull/14203
synced_at: 2026-01-12T15:55:47Z
```

# FBT001: exclude boolean operators

---

_@randolf-scholz_

Fixes #14202

## Summary

Exclude rule FBT001 for boolean operators.

## Test Plan

Updated existing `FBT.py` test.

---

_Comment by @charliermarsh on 2024-11-09 02:11_

Should we be ignoring all dunders?

---

_Comment by @charliermarsh on 2024-11-09 03:13_

We have an `is_known_dunder_method` method that we could use for that.

---

_Comment by @github-actions[bot] on 2024-11-10 11:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @randolf-scholz on 2024-11-10 11:13_

@charliermarsh I updated using `is_known_dunder_method` (this required making `crate::rules::flake8_boolean_trap::helpers` public).

However, I am not sure if this is the best approach. For one, there is no official list of dunder methods from python's side, and what is considered a known dunder method can change from one version to the next.

Maybe a simpler approach would be to just exclude dunder methods in general? This would be easier to document/teach as well.

---

_Comment by @randolf-scholz on 2024-11-10 11:16_

The `is_known_dunder_method` also contains `__init__` and `__new__`, which are probably the two most important functions for `FBT001` to check.

---

_Marked ready for review by @randolf-scholz on 2024-11-10 12:02_

---

_Comment by @dylwil3 on 2024-11-10 13:52_

I think "all dunder methods except init and new" should be easy to maintain and explain.

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/flake8_boolean_trap/FBT.py`:1 on 2024-11-10 13:53_

Could you add some test cases where the `bool` appears as part of a union type? (They'll certainly pass, just want to prevent future regressions)

---

_@dylwil3 reviewed on 2024-11-10 13:55_

---

_@randolf-scholz reviewed on 2024-11-10 18:02_

---

_Review comment by @randolf-scholz on `crates/ruff_linter/resources/test/fixtures/flake8_boolean_trap/FBT.py`:1 on 2024-11-10 18:02_

I added a test, and also one for overloads.

---

_@charliermarsh approved on 2024-11-10 22:36_

Thanks -- this looks reasonable to me.

---

_Label `rule` added by @charliermarsh on 2024-11-10 22:36_

---

_Merged by @charliermarsh on 2024-11-10 22:40_

---

_Closed by @charliermarsh on 2024-11-10 22:40_

---
