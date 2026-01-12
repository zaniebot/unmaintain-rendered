```yaml
number: 20810
title: "[`fastapi`] Handle ellipsis defaults in FAST002 autofix (`FAST002`)"
type: pull_request
state: merged
author: hengky-kurniawan-1
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: main
created_at: 2025-10-11T12:27:16Z
updated_at: 2025-10-21T02:52:44Z
url: https://github.com/astral-sh/ruff/pull/20810
synced_at: 2026-01-12T15:57:10Z
```

# [`fastapi`] Handle ellipsis defaults in FAST002 autofix (`FAST002`)

---

_@hengky-kurniawan-1_

## Summary

Implement handling of ellipsis (`...`) defaults in the `FAST002` autofix to correctly differentiate between required and optional parameters in FastAPI route definitions.

Previously, the autofix did not properly handle cases where parameters used `...` as a default value (to indicate required parameters). This could lead to incorrect transformations when applying the autofix.

This change updates the `FAST002` autofix logic to:
- Correctly recognize `...` as a valid FastAPI required default.
- Preserve the semantics of required parameters while still applying other autofix improvements.
- Avoid incorrectly substituting or removing ellipsis defaults.

Fixes https://github.com/astral-sh/ruff/issues/20800

## Test Plan

Added a new test fixture at:
```crates/ruff_linter/resources/test/fixtures/fastapi/FAST002_2.py```

---

_Label `bug` added by @ntBre on 2025-10-15 18:20_

---

_Label `fixes` added by @ntBre on 2025-10-15 18:20_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/fastapi/FAST002_2.py`:108 on 2025-10-15 19:01_

I think we could probably omit these cases since `Depends` and `Security` don't take ellipsis arguments. Is that right?

The names feel slightly misleading to me since there aren't ellipses in these examples, and we have other tests for `Depends` and `Security` in FAST002_1.py.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/fastapi/FAST002_2.py`:118 on 2025-10-15 19:03_

Do we have a test case where we bail out  because we removed a default? I think swapping the order of these two parameters to trigger that behavior might be more interesting, unless I missed another case like that.

---

_@ntBre reviewed on 2025-10-15 19:05_

Thank you! This looks great. I just had a couple of small ideas/suggestions about the tests.

---

_Comment by @ntBre on 2025-10-15 19:06_

It looks like the linux test failure was just a transient issue. The only real CI problem is the formatting.

---

_Comment by @hengky-kurniawan-1 on 2025-10-16 00:57_

Thanks for the review! I’ll update it soon.

---

_@hengky-kurniawan-1 reviewed on 2025-10-16 01:02_

---

_Review comment by @hengky-kurniawan-1 on `crates/ruff_linter/resources/test/fixtures/fastapi/FAST002_2.py`:118 on 2025-10-16 01:02_

Interesting thought! I’ll update it to include that case soon.

---

_Review comment by @hengky-kurniawan-1 on `crates/ruff_linter/resources/test/fixtures/fastapi/FAST002_2.py`:108 on 2025-10-16 01:07_

Oh right, I’ll remove those cases.

---

_@hengky-kurniawan-1 reviewed on 2025-10-16 01:07_

---

_Review requested from @ntBre by @hengky-kurniawan-1 on 2025-10-16 02:12_

---

_@ntBre approved on 2025-10-20 21:49_

Thank you! We just need to update one more minor thing in the snapshots.

I tried to push a commit, but GitHub said I didn't have permission. Sorry for the trouble!

---

_@ntBre reviewed on 2025-10-20 21:50_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/fastapi/snapshots/ruff_linter__rules__fastapi__tests__fast-api-non-annotated-dependency_FAST002_2.py.snap`:273 on 2025-10-20 21:50_

It looks like this comment is just outdated. The new comment in the .py file looks perfect.

---

_@hengky-kurniawan-1 reviewed on 2025-10-21 01:49_

---

_Review comment by @hengky-kurniawan-1 on `crates/ruff_linter/src/rules/fastapi/snapshots/ruff_linter__rules__fastapi__tests__fast-api-non-annotated-dependency_FAST002_2.py.snap`:273 on 2025-10-21 01:49_

Nice catch.
I’ve updated the snapshot accordingly. Thanks!

---

_Merged by @ntBre on 2025-10-21 02:47_

---

_Closed by @ntBre on 2025-10-21 02:47_

---

_Comment by @github-actions[bot] on 2025-10-21 02:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
