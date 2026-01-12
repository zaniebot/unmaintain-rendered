```yaml
number: 461
title: Fix uppercase and lowercase check
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: fix-casing-check
created_at: 2022-10-22T08:02:17Z
updated_at: 2022-10-22T16:49:14Z
url: https://github.com/astral-sh/ruff/pull/461
synced_at: 2026-01-12T15:55:04Z
```

# Fix uppercase and lowercase check

---

_@harupy_

`string.chars().all(|c| c.is_uppercase|is_lowercase())` retruns `False` when `string` contains `_`. This PR fixes it.

---

_@harupy reviewed on 2022-10-22 08:12_

---

_Review comment by @harupy on `src/pep8_naming/checks.rs`:127 on 2022-10-22 08:12_

This implementation is based on cpython's is_lower:

https://github.com/python/cpython/blob/7108bdf27c7a460cf83c4a01dea54ae4591d8aea/Objects/bytes_methods.c#L184

---

_@charliermarsh reviewed on 2022-10-22 16:05_

---

_Review comment by @charliermarsh on `src/pep8_naming/checks.rs`:127 on 2022-10-22 16:05_

Awesome, can we add a test or two for these functions?

---

_Merged by @charliermarsh on 2022-10-22 16:49_

---

_Closed by @charliermarsh on 2022-10-22 16:49_

---
