```yaml
number: 15110
title: "Add test cases for `find_uv_bin`"
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/test-find-uv-bin
created_at: 2025-08-06T15:19:57Z
updated_at: 2025-08-07T21:46:16Z
url: https://github.com/astral-sh/uv/pull/15110
synced_at: 2026-01-10T06:44:33Z
```

# Add test cases for `find_uv_bin`

---

_Pull request opened by @zanieb on 2025-08-06 15:19_

Adds test cases to unblock

- https://github.com/astral-sh/uv/pull/14181
- https://github.com/astral-sh/uv/pull/14182
- https://github.com/astral-sh/uv/pull/14184
- https://github.com/tox-dev/pre-commit-uv/issues/70

We use a package with a symlink to the Python module to get a mock installation of uv without building (or packaging) the uv binary. This lets us test real patterns like `uv pip install --prefix` without encoding logic about where things are placed during those installs.

---

_Label `testing` added by @zanieb on 2025-08-06 15:19_

---

_@zanieb reviewed on 2025-08-06 15:35_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:158 on 2025-08-06 15:35_

This allows copying broken symlinks.

And yes, `robocopy` uses a non-zero exit code for success.

---

_Marked ready for review by @zanieb on 2025-08-07 03:55_

---

_Review comment by @konstin on `crates/uv/tests/it/python_module.rs`:28 on 2025-08-07 09:03_

Why are we using `with_filter` here but `let mut filters = context.filters();` in the rest of the tests?

---

_@konstin approved on 2025-08-07 09:04_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_module.rs`:28 on 2025-08-07 12:13_

I think it's a better pattern — I intend to update the other cases later.

---

_@zanieb reviewed on 2025-08-07 12:13_

---

_Merged by @zanieb on 2025-08-07 12:14_

---

_Closed by @zanieb on 2025-08-07 12:14_

---

_Branch deleted on 2025-08-07 12:14_

---
