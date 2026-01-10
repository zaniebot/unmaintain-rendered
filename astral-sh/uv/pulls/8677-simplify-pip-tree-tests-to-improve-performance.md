```yaml
number: 8677
title: "Simplify `pip tree` tests to improve performance"
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/tree
created_at: 2024-10-29T18:18:45Z
updated_at: 2024-10-29T18:59:28Z
url: https://github.com/astral-sh/uv/pull/8677
synced_at: 2026-01-10T12:54:15Z
```

# Simplify `pip tree` tests to improve performance

---

_Pull request opened by @charliermarsh on 2024-10-29 18:18_

## Summary

These use really heavy test packages, like SciPy, NumPy, scikit-learn -- and packages with large dependency trees, like packse.

I removed a few redundant tests, and replaced the tests with smaller packages.

Closes https://github.com/astral-sh/uv/issues/8674.


---

_@charliermarsh reviewed on 2024-10-29 18:19_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_tree.rs`:500 on 2024-10-29 18:19_

I don't think these "complex" tests are worth it. We have to install the whole tree here, and they aren't testing any new behavior.

---

_Label `testing` added by @charliermarsh on 2024-10-29 18:20_

---

_@zanieb approved on 2024-10-29 18:24_

---

_Merged by @charliermarsh on 2024-10-29 18:59_

---

_Closed by @charliermarsh on 2024-10-29 18:59_

---

_Branch deleted on 2024-10-29 18:59_

---
