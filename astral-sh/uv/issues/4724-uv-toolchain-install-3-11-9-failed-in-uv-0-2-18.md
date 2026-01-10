---
number: 4724
title: "``uv toolchain install 3.11.9``  failed in uv 0.2.18 on Windows 11 23H2."
type: issue
state: closed
author: FishAlchemist
labels:
  - bug
  - windows
assignees: []
created_at: 2024-07-02T09:15:11Z
updated_at: 2024-07-02T13:21:16Z
url: https://github.com/astral-sh/uv/issues/4724
synced_at: 2026-01-10T01:23:40Z
---

# ``uv toolchain install 3.11.9``  failed in uv 0.2.18 on Windows 11 23H2.

---

_Issue opened by @FishAlchemist on 2024-07-02 09:15_

### Version
```powershell
uv 0.2.18 (13b0beb56 2024-06-29)
```
### Command
```powershell
uv toolchain install 3.11 --verbose
```
#### Verbose Output
```
DEBUG uv 0.2.18
warning: `uv toolchain install` is experimental and may change without warning.
Looking for toolchain Python 3.11 (any-3.11-any-any-any)
DEBUG Using request timeout of 30s
Downloading cpython-3.11.9-windows-x86_64-none
DEBUG Downloading https://github.com/indygreg/python-build-standalone/releases/download/20240415/cpython-3.11.9%2B20240415-x86_64-pc-windows-msvc-shared-pgo-full.tar.zst to temporary location C:\Users\user_name\AppData\Roaming\uv\data\toolchains\.tmp14KKGA
DEBUG Extracting cpython-3.11.9%2B20240415-x86_64-pc-windows-msvc-shared-pgo-full.tar.zst
DEBUG Moving C:\Users\user_name\AppData\Roaming\uv\data\toolchains\.tmp14KKGA\python to AppData\Roaming\uv\data\toolchains\cpython-3.11.9-windows-x86_64-none
Installed Python 3.11.9 to AppData\Roaming\uv\data\toolchains\cpython-3.11.9-windows-x86_64-none
error: failed to create file `C:\Users\user_name\AppData\Roaming\uv\data\toolchains\cpython-3.11.9-windows-x86_64-none\install\Lib\python3.11\EXTERNALLY-MANAGED`
    Caused by: The system cannot find the path specified. (os error 3) 
 ```
If it is a known bug, should it be reported in Windows?


---

_Label `bug` added by @charliermarsh on 2024-07-02 11:51_

---

_Label `windows` added by @charliermarsh on 2024-07-02 11:51_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-02 12:31_

---

_Comment by @charliermarsh on 2024-07-02 12:31_

Looks like our bug.

---

_Referenced in [astral-sh/uv#4727](../../astral-sh/uv/pulls/4727.md) on 2024-07-02 12:36_

---

_Closed by @charliermarsh on 2024-07-02 13:21_

---
