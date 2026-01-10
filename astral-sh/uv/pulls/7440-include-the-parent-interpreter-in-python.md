```yaml
number: 7440
title: "Include the parent interpreter in Python discovery when `--system` is used"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/parent-system-interpreter
created_at: 2024-09-16T20:21:59Z
updated_at: 2024-09-17T03:51:51Z
url: https://github.com/astral-sh/uv/pull/7440
synced_at: 2026-01-10T12:53:47Z
```

# Include the parent interpreter in Python discovery when `--system` is used

---

_Pull request opened by @zanieb on 2024-09-16 20:21_

Closes https://github.com/astral-sh/uv/issues/7417

Tested with this epic blurb

```
❯ uv pip install --python /Users/zb/.local/share/uv/python/cpython-3.13.0rc2-macos-aarch64-none/bin/python3 . --break-system-packages --reinstall
Resolved 1 package in 0.91ms
   Built uv @ file:///Users/zb/workspace/uv
Prepared 1 package in 2m 48s
Uninstalled 1 package in 1ms
Installed 1 package in 1ms
 - uv==0.4.10
 + uv==0.4.10 (from file:///Users/zb/workspace/uv)
❯ /Users/zb/.local/share/uv/python/cpython-3.13.0rc2-macos-aarch64-none/bin/python3 -m uv pip install --system -v httpx
DEBUG uv 0.4.10
DEBUG Searching for Python interpreter in system path
DEBUG Found `cpython-3.13.0rc2-macos-aarch64-none` at `/Users/zb/.local/share/uv/python/cpython-3.13.0rc2-macos-aarch64-none/bin/python3` (parent interpreter)
DEBUG Using Python 3.13.0rc2 environment at /Users/zb/.local/share/uv/python/cpython-3.13.0rc2-macos-aarch64-none/bin/python3
```

---

_Label `bug` added by @zanieb on 2024-09-16 20:22_

---

_Review requested from @charliermarsh by @zanieb on 2024-09-16 20:41_

---

_@zanieb reviewed on 2024-09-16 20:47_

---

_Review comment by @zanieb on `crates/uv-python/src/lib.rs`:1251 on 2024-09-16 20:47_

Test case added in #7439 

---

_Merged by @zanieb on 2024-09-17 03:51_

---

_Closed by @zanieb on 2024-09-17 03:51_

---

_Branch deleted on 2024-09-17 03:51_

---
