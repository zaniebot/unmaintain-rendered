```yaml
number: 2031
title: "Accept (e.g.) `'python3.8'` as `--python` argument"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/py
created_at: 2024-02-28T02:35:27Z
updated_at: 2024-02-28T14:48:52Z
url: https://github.com/astral-sh/uv/pull/2031
synced_at: 2026-01-10T14:54:43Z
```

# Accept (e.g.) `'python3.8'` as `--python` argument

---

_Pull request opened by @charliermarsh on 2024-02-28 02:35_

## Summary

This PR aligns the `uv pip install --python` flag with the `uv venv --python` flag, such that the former now accepts binary names and Python versions by way of using the same `find_requested_python` method under the hood.


---

_Review requested from @MichaReiser by @charliermarsh on 2024-02-28 02:35_

---

_Review requested from @konstin by @charliermarsh on 2024-02-28 02:35_

---

_Renamed from "Accept (e.g.) 'python3.8' as --python argument" to "Accept (e.g.) `'python3.8'` as `--python` argument" by @charliermarsh on 2024-02-28 02:35_

---

_Label `cli` added by @charliermarsh on 2024-02-28 02:35_

---

_Review requested from @zanieb by @charliermarsh on 2024-02-28 02:54_

---

_Review comment by @konstin on `crates/uv/src/main.rs`:451 on 2024-02-28 09:15_

Patch versions are supported, `uv pip install --python 3.11.6` passes, `uv pip install --python 3.11.5` errors.
 

---

_Comment by @konstin on 2024-02-28 09:21_

We should improve the error message (separate PR?):
```
$ uv pip install --python 3.11.6 black
error: failed to create file `/usr/.lock`
  Caused by: Permission denied (os error 13)
```

We should fail due to `/usr/lib/python3.11/EXTERNALLY-MANAGED`. I also think we need to place the lockfile somewhere else in those cases, `/usr/.lock` sounds like a dangerously generic location.

---

_@konstin approved on 2024-02-28 09:21_

---

_@charliermarsh reviewed on 2024-02-28 14:48_

---

_Review comment by @charliermarsh on `crates/uv/src/main.rs`:451 on 2024-02-28 14:48_

Ah ok. This was copied over from the other messages in the CLI. It must be stale. I'll fix it separately.

---

_Merged by @charliermarsh on 2024-02-28 14:48_

---

_Closed by @charliermarsh on 2024-02-28 14:48_

---

_Branch deleted on 2024-02-28 14:48_

---
