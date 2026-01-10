```yaml
number: 10721
title: Avoid attempting to patch macOS dylib for non-macOS installs
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ligs
created_at: 2025-01-17T19:09:41Z
updated_at: 2025-01-17T19:21:33Z
url: https://github.com/astral-sh/uv/pull/10721
synced_at: 2026-01-10T11:45:06Z
```

# Avoid attempting to patch macOS dylib for non-macOS installs

---

_Pull request opened by @charliermarsh on 2025-01-17 19:09_

## Summary

For example, `cargo run python install cpython-3.12.8-linux-x86_64_v3-gnu` (on macOS) shouldn't attempt to patch the dylib. At present, it leads to this warning:

```
warning: Failed to patch the install name of the dynamic library for /Users/crmarsh/.local/share/uv/python/cpython-3.12.8-linux-x86_64_v3-gnu/bin/python3.12. This may cause issues when building Python native extensions.
Underlying error: Failed to update the install name of the Python dynamic library located at `/Users/crmarsh/.local/share/uv/python/cpython-3.12.8-linux-x86_64_v3-gnu/lib/libpython3.12.dylib`
```


---

_Label `bug` added by @charliermarsh on 2025-01-17 19:09_

---

_@zanieb approved on 2025-01-17 19:10_

---

_Merged by @charliermarsh on 2025-01-17 19:21_

---

_Closed by @charliermarsh on 2025-01-17 19:21_

---

_Branch deleted on 2025-01-17 19:21_

---
