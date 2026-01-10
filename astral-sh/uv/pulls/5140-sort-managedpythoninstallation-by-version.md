```yaml
number: 5140
title: "Sort `ManagedPythonInstallation` by version"
type: pull_request
state: merged
author: j178
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: py-version
created_at: 2024-07-17T03:55:34Z
updated_at: 2024-07-18T05:38:54Z
url: https://github.com/astral-sh/uv/pull/5140
synced_at: 2026-01-10T13:42:52Z
```

# Sort `ManagedPythonInstallation` by version

---

_Pull request opened by @j178 on 2024-07-17 03:55_

## Summary
Resolves #5139

`PythonInstallationKey` was sorted as a string, which caused `3.8` to appear before `3.11`. This update changes the sorting of `PythonInstallationKey` to be a descending order by version.

## Test Plan
```sh
$ cargo run -- python install 3.8 3.12
$ cargo run -- tool run -v python -V
DEBUG uv 0.2.25
warning: `uv tool run` is experimental and may change without warning.
DEBUG Searching for Python interpreter in managed installations, system path, or `py` launcher
DEBUG Searching for managed installations at `C:\Users\xx\AppData\Roaming\uv\data\python`
DEBUG Found managed Python `cpython-3.12.3-windows-x86_64-none`
DEBUG Found cpython 3.12.3 at `C:\Users\xx\AppData\Roaming\uv\data\python\cpython-3.12.3-windows-x86_64-none\install\python.exe` (managed installations)
DEBUG Using request timeout of 30s
DEBUG Using request timeout of 30s
DEBUG Acquired lock for `C:\Users\nigel\AppData\Roaming\uv\data\tools`
DEBUG Using existing environment for tool `httpx`: C:\Users\xx\AppData\Roaming\uv\data\tools\httpx
DEBUG Using existing tool `httpx`
DEBUG Running `httpx -v`
```


---

_Renamed from "Sort ManagedPythonInstallation by version" to "Sort `ManagedPythonInstallation` by version" by @j178 on 2024-07-17 03:55_

---

_Review requested from @zanieb by @konstin on 2024-07-17 07:04_

---

_@zanieb approved on 2024-07-17 14:47_

Thanks!

---

_Label `bug` added by @zanieb on 2024-07-17 14:47_

---

_Label `preview` added by @zanieb on 2024-07-17 14:47_

---

_Merged by @zanieb on 2024-07-17 14:48_

---

_Closed by @zanieb on 2024-07-17 14:48_

---

_Branch deleted on 2024-07-18 05:38_

---
