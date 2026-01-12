```yaml
number: 1713
title: Implement autofix for PT009
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: autofix-PT009
created_at: 2023-01-07T11:05:06Z
updated_at: 2023-01-07T17:28:25Z
url: https://github.com/astral-sh/ruff/pull/1713
synced_at: 2026-01-12T05:36:32Z
```

# Implement autofix for PT009

---

_Pull request opened by @harupy on 2023-01-07 11:05_

Implements autofix for PT009

---

_Review comment by @charliermarsh on `src/flake8_pytest_style/plugins/unittest_assert.rs`:406 on 2023-01-07 12:35_

You can use `ast::helpers::create_expr` to avoid having to provide the `Location::default()` args, although maybe that's not that helpful of a utility.

---

_@charliermarsh reviewed on 2023-01-07 12:35_

---

_@harupy reviewed on 2023-01-07 13:29_

---

_Review comment by @harupy on `src/flake8_pytest_style/plugins/unittest_assert.rs`:406 on 2023-01-07 13:29_

Didn't know that method! Updated

---

_Review comment by @harupy on `src/flake8_pytest_style/plugins/unittest_assert.rs`:128 on 2023-01-07 14:04_

Maybe it's fine to fix `assertSetEqual(s1, s2)` to `assert s1 == s2`.

---

_@harupy reviewed on 2023-01-07 14:04_

---

_Merged by @charliermarsh on 2023-01-07 17:28_

---

_Closed by @charliermarsh on 2023-01-07 17:28_

---
