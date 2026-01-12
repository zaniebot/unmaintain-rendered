```yaml
number: 14949
title: "[`ruff`]  Skip SQLModel base classes for `mutable-class-default` (`RUF012`)"
type: pull_request
state: merged
author: krishnan-chandra
labels:
  - rule
assignees: []
merged: true
base: main
head: ruf012-sqlmodel-exemption
created_at: 2024-12-12T23:58:32Z
updated_at: 2024-12-13T12:10:08Z
url: https://github.com/astral-sh/ruff/pull/14949
synced_at: 2026-01-12T15:55:49Z
```

# [`ruff`]  Skip SQLModel base classes for `mutable-class-default` (`RUF012`)

---

_@krishnan-chandra_

## Summary

Closes https://github.com/astral-sh/ruff/issues/14892, by adding `sqlmodel.SQLModel` to the list of classes with default copy semantics. 

## Test Plan

Added a test into `RUF012.py` containing the example from the original issue.


---

_Comment by @github-actions[bot] on 2024-12-13 00:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/ruff/RUF012.py`:1 on 2024-12-13 01:57_

Great! Could you add a couple more test cases?

- one where you import something called `SQLModel` but _not_ from `sqlmodel`? (you may have to do it inside a function so it's scope doesn't mess with the scope where you already have an import)
- one where the mutable default is initialized differently, like with `list()`, say

---

_@dylwil3 requested changes on 2024-12-13 01:58_

Thank you so much! Just one request for some more tests and then it looks good to me

---

_Label `rule` added by @dylwil3 on 2024-12-13 01:59_

---

_Renamed from "RUF012 exemption for SQLModel base class" to "[`ruff`]  Skip SQLModel base classes for `mutable-class-default` (`RUF012`)" by @dylwil3 on 2024-12-13 02:00_

---

_@krishnan-chandra reviewed on 2024-12-13 03:29_

---

_Review comment by @krishnan-chandra on `crates/ruff_linter/resources/test/fixtures/ruff/RUF012.py`:1 on 2024-12-13 03:29_

Think I got both cases covered now + updated the snapshot, let me know if you think differently or if you can think of anything else to be covered.

---

_Review requested from @dylwil3 by @krishnan-chandra on 2024-12-13 03:33_

---

_@dylwil3 approved on 2024-12-13 04:18_

LGTM - thank you again for your contribution!

---

_Merged by @dylwil3 on 2024-12-13 04:19_

---

_Closed by @dylwil3 on 2024-12-13 04:19_

---

_Branch deleted on 2024-12-13 12:10_

---
