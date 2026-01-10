```yaml
number: 13023
title: "`workspace_lock_idempotence_root_workspace` test flake on Windows"
type: issue
state: closed
author: zanieb
labels:
  - testing
  - ci-flake
assignees: []
created_at: 2025-04-21T20:04:11Z
updated_at: 2025-06-03T14:39:06Z
url: https://github.com/astral-sh/uv/issues/13023
synced_at: 2026-01-10T03:41:47Z
```

# `workspace_lock_idempotence_root_workspace` test flake on Windows

---

_Issue opened by @zanieb on 2025-04-21 20:04_

https://github.com/astral-sh/uv/actions/runs/14580087321/job/40894722667?pr=10923

```
──── STDERR:             uv::it workspace::workspace_lock_idempotence_root_workspace

thread 'workspace::workspace_lock_idempotence_root_workspace' panicked at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb\library\core\src\ops\function.rs:250:5:
Unexpected failure.
code=-1073741819
stderr=
Using CPython 3.12.9 interpreter at: C:\\Users\\runneradmin\\AppData\\Roaming\\uv\\python\\cpython-3.12.9-windows-x86_64-none\\python.exe
Resolved 5 packages in 128ms

command=`"E:\\uv\\target\\debug\\uv.exe" "lock" "--cache-dir" "E:\\uv-tmp\\.tmpHOGyRl\\cache"`
code=-1073741819
stdout=""
stderr=
Using CPython 3.12.9 interpreter at: C:\\Users\\runneradmin\\AppData\\Roaming\\uv\\python\\cpython-3.12.9-windows-x86_64-none\\python.exe
Resolved 5 packages in 128ms


note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

---

_Label `testing` added by @zanieb on 2025-04-21 20:04_

---

_Label `ci-flake` added by @zanieb on 2025-05-30 21:16_

---

_Comment by @konstin on 2025-06-03 14:37_

Duplicate of #13812

---

_Closed by @konstin on 2025-06-03 14:37_

---

_Comment by @zanieb on 2025-06-03 14:39_

Brutal, closing the old for the new? :D

---
