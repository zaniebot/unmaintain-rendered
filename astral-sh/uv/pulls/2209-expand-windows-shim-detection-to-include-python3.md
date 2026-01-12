```yaml
number: 2209
title: "Expand Windows shim detection to include `python3.12.exe`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/win
created_at: 2024-03-05T17:53:13Z
updated_at: 2024-03-05T18:25:06Z
url: https://github.com/astral-sh/uv/pull/2209
synced_at: 2026-01-12T16:04:55Z
```

# Expand Windows shim detection to include `python3.12.exe`

---

_@charliermarsh_

## Summary

Our Windows shim detection wasn't catching shims like `python3.12.exe`.

Closes #2208.

## Test Plan

Installed Python 3.12 via the Windows Store; verified that `cargo run venv --python 3.12` failed before but passes after this change.


---

_Label `bug` added by @charliermarsh on 2024-03-05 17:53_

---

_Label `windows` added by @charliermarsh on 2024-03-05 17:53_

---

_Marked ready for review by @charliermarsh on 2024-03-05 17:53_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/python_query.rs`:418 on 2024-03-05 17:54_

I structured this to look similar to `is_windows_store_python`.

---

_@charliermarsh reviewed on 2024-03-05 17:54_

---

_Merged by @charliermarsh on 2024-03-05 18:25_

---

_Closed by @charliermarsh on 2024-03-05 18:25_

---

_Branch deleted on 2024-03-05 18:25_

---
