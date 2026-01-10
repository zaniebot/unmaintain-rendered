```yaml
number: 18631
title: "[`ruff`] Validate arguments before offering a fix (`RUF056`)"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: brent/fix-ruf056
created_at: 2025-06-11T17:52:29Z
updated_at: 2025-06-13T23:10:18Z
url: https://github.com/astral-sh/ruff/pull/18631
synced_at: 2026-01-10T18:45:04Z
```

# [`ruff`] Validate arguments before offering a fix (`RUF056`)

---

_Pull request opened by @ntBre on 2025-06-11 17:52_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/18628 by avoiding a fix if there are "unknown" arguments, including any keyword arguments and more than the expected 2 positional arguments.

I'm a bit on the fence here because it also seems reasonable to avoid a diagnostic at all. Especially in the final test case I added (`not my_dict.get(default=False)`), the hint suggesting to remove `default=False` seems pretty misleading. At the same time, I guess the diagnostic at least calls attention to the call site, which could help to fix the missing argument bug too.

As I commented on the issue, I double-checked that keyword arguments are invalid as far back as Python 3.8, even though the positional-only marker was only added to the [docs](https://docs.python.org/3.11/library/stdtypes.html#dict.get) in 3.12 (link is to 3.11, showing its absence).

## Test Plan

New tests derived from the bug report

## Stabilization

This was planned to be stabilized in 0.12, and the bug is less severe than some others, but if there's nobody opposed, I will plan **not to stabilize** this one for now.


---

_Label `bug` added by @ntBre on 2025-06-11 17:52_

---

_Label `fixes` added by @ntBre on 2025-06-11 17:52_

---

_Review requested from @MichaReiser by @ntBre on 2025-06-11 17:52_

---

_Comment by @github-actions[bot] on 2025-06-11 17:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/RUF056.py`:166 on 2025-06-12 06:40_

Should we remove the tests on line 152 or change them, or group them?

---

_@MichaReiser approved on 2025-06-12 06:43_

makes sense. I agree that we could also decide to not raise a diagnostic here because it could hide a more important type checker diagnostic that the keyword argument isn't allowed. 

I'm fine with either change/fix. 

---

_Merged by @ntBre on 2025-06-13 23:07_

---

_Closed by @ntBre on 2025-06-13 23:07_

---

_Branch deleted on 2025-06-13 23:07_

---
