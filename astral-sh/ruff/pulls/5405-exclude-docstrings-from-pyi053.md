```yaml
number: 5405
title: Exclude docstrings from PYI053
type: pull_request
state: merged
author: intgr
labels:
  - bug
assignees: []
merged: true
base: main
head: PYI053-exclude-docstrings
created_at: 2023-06-27T21:55:04Z
updated_at: 2023-06-28T00:19:21Z
url: https://github.com/astral-sh/ruff/pull/5405
synced_at: 2026-01-12T15:55:18Z
```

# Exclude docstrings from PYI053

---

_@intgr_

## Summary

The `Y053` rule of `flake8-pyi` ignores docstrings, it only triggers on other string literals.

The separate `Y021/PYI021` rule exists to disallow docstrings.

## Test Plan

Added some `# OK` test cases to `PYI053.py(i)` files.


---

_Comment by @intgr on 2023-06-27 22:28_

For some reason the "PR Check Comment" workflow keeps failing on my branch: https://github.com/astral-sh/ruff/actions/runs/5395179324/jobs/9797343880

---

_Comment by @charliermarsh on 2023-06-27 23:49_

All good, this seems correct to me, will merge tonight. Thanks!

---

_Label `bug` added by @charliermarsh on 2023-06-28 00:13_

---

_Merged by @charliermarsh on 2023-06-28 00:19_

---

_Closed by @charliermarsh on 2023-06-28 00:19_

---
