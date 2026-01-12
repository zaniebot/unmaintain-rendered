```yaml
number: 7741
title: Fix PLE251 rules with f-string escaping
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-PLE251-escaping
created_at: 2023-10-01T15:35:56Z
updated_at: 2023-10-02T08:57:48Z
url: https://github.com/astral-sh/ruff/pull/7741
synced_at: 2026-01-12T15:55:24Z
```

# Fix PLE251 rules with f-string escaping

---

_@konstin_

**Summary** The `value` of the `FStringMiddle` for `f"""}}ab"""` is `}ab`, i.e. the curly brace escaping is decoded. If we iterate over string this gives us false indices causing exploding fixes for PLE251 rules (PLE2510, PLE2512, PLE2513, PLE2514, PLE2515). Instead, we now use the source range.

Handles https://github.com/astral-sh/ruff/issues/7455#issuecomment-1741998106
Handles https://github.com/astral-sh/ruff/issues/7455#issuecomment-1741998256

**Test Plan** Minimized fuzzing cases as fixtures.


---

_Label `bug` added by @konstin on 2023-10-01 15:35_

---

_Comment by @github-actions[bot] on 2023-10-01 15:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@charliermarsh reviewed on 2023-10-01 17:59_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/invalid_string_characters.rs`:185 on 2023-10-01 17:59_

Nit: this can be `locator.slice(range)`, right? So perhaps the above should just be changed to `Tok::String { .. } | Tok::FStringMiddle { .. } => ...`?

---

_@charliermarsh approved on 2023-10-01 17:59_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/invalid_string_characters.rs`:185 on 2023-10-01 18:07_

Yes, this is correct.

---

_@dhruvmanila reviewed on 2023-10-01 18:07_

---

_@dhruvmanila approved on 2023-10-01 18:08_

---

_Comment by @dhruvmanila on 2023-10-01 18:08_

Thanks!

---

_Merged by @konstin on 2023-10-02 08:43_

---

_Closed by @konstin on 2023-10-02 08:43_

---

_Branch deleted on 2023-10-02 08:43_

---
