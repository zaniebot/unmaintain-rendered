```yaml
number: 2237
title: "test: pip_list: Fix replacement pattern computation"
type: pull_request
state: merged
author: mgorny
labels:
  - testing
assignees: []
merged: true
base: main
head: pip-list-len
created_at: 2024-03-06T10:03:54Z
updated_at: 2024-03-06T14:13:31Z
url: https://github.com/astral-sh/uv/pull/2237
synced_at: 2026-01-10T14:54:43Z
```

# test: pip_list: Fix replacement pattern computation

---

_Pull request opened by @mgorny on 2024-03-06 10:03_

## Summary

Fix computing replacements pattern for pip_list tests to count characters in the original directory string rather than the regex::escape'd string.  The latter yields incorrect results if the workspace path contains characters such as `-` or `.`.

Fixes #2232

## Test Plan

`cargo test --test pip_list` in a directory named `uv-test` to provoke the bug.


---

_@charliermarsh approved on 2024-03-06 13:32_

Thank you!

---

_Label `testing` added by @charliermarsh on 2024-03-06 13:32_

---

_Merged by @charliermarsh on 2024-03-06 13:32_

---

_Closed by @charliermarsh on 2024-03-06 13:32_

---

_Branch deleted on 2024-03-06 14:13_

---

_Comment by @mgorny on 2024-03-06 14:13_

Thanks!

---
