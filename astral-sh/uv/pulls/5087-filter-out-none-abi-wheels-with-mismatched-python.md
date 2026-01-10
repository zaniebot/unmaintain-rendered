```yaml
number: 5087
title: Filter out none ABI wheels with mismatched Python versions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/requires-python-filter
created_at: 2024-07-16T01:28:31Z
updated_at: 2024-07-16T10:22:35Z
url: https://github.com/astral-sh/uv/pull/5087
synced_at: 2026-01-10T13:42:52Z
```

# Filter out none ABI wheels with mismatched Python versions

---

_Pull request opened by @charliermarsh on 2024-07-16 01:28_

## Summary

`echo "torch==1.10.0" | cargo run pip compile - -p 3.12 --no-deps` now correctly fails. Previously, we were accepting the wheel `torch-1.10.0-cp36-none-macosx_10_9_x86_64.whl` as compatible with Python 3.10 due to the `none` ABI.

Closes https://github.com/astral-sh/uv/issues/5085.


---

_Marked ready for review by @charliermarsh on 2024-07-16 01:28_

---

_Review requested from @konstin by @charliermarsh on 2024-07-16 01:41_

---

_Merged by @charliermarsh on 2024-07-16 01:41_

---

_Closed by @charliermarsh on 2024-07-16 01:41_

---

_Branch deleted on 2024-07-16 01:41_

---

_Label `bug` added by @charliermarsh on 2024-07-16 01:41_

---

_@konstin reviewed on 2024-07-16 10:22_

Good catch

---
