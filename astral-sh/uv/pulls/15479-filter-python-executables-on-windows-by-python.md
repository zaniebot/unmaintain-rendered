```yaml
number: 15479
title: Filter Python executables on Windows by Python version metadata
type: pull_request
state: open
author: sunfkny
labels: []
assignees: []
base: main
head: filter-win
created_at: 2025-08-24T11:59:16Z
updated_at: 2025-08-24T16:16:47Z
url: https://github.com/astral-sh/uv/pull/15479
synced_at: 2026-01-10T06:44:33Z
```

# Filter Python executables on Windows by Python version metadata

---

_Pull request opened by @sunfkny on 2025-08-24 11:59_

## Summary

Fix #13927

## Test Plan

Tested by installing multiple Python versions on Windows, adjusting the PATH order, then running `cargo run --bin uv -- run -p 3.12 -v python -V` and verifying in the debug logs that mismatched interpreters were skipped and the correct Python 3.12 executable was selected.

```
> where.exe python
C:\Users\root\AppData\Local\Programs\Python\Python314\python.exe
C:\Users\root\AppData\Local\Programs\Python\Python313\python.exe
C:\Users\root\AppData\Local\Programs\Python\Python312\python.exe

> cargo run --bin uv -- run -p 3.12 -v python -V
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.92s
     Running `target\debug\uv.exe run -p 3.12 -v python -V`
DEBUG uv 0.8.13 (6819ce2b5 2025-08-24)
DEBUG Found project root: `C:\Code\uv`
DEBUG Project `uv` is marked as unmanaged
DEBUG No project found; searching for Python interpreter
DEBUG Searching for Python 3.12 in virtual environments, managed installations, search path, or registry
DEBUG Searching for managed installations at `C:\Users\root\AppData\Roaming\uv\python`
DEBUG Skipping managed installation `cpython-3.14.0rc2-windows-x86_64-none`: does not satisfy `3.12`
DEBUG Skipping managed installation `cpython-3.13.7-windows-x86_64-none`: does not satisfy `3.12`
DEBUG Skipping interpreter at `C:\Users\root\AppData\Local\Programs\Python\Python314\python.exe`: PE version `3.14.0rc2` does not match `3.12`
DEBUG Skipping interpreter at `C:\Users\root\AppData\Local\Programs\Python\Python313\python.exe`: PE version `3.13.7` does not match `3.12`
DEBUG Found `cpython-3.12.10-windows-x86_64-none` at `C:\Users\root\AppData\Local\Programs\Python\Python312\python.exe` (first executable in the search path)
DEBUG Using Python 3.12.10 interpreter at: C:\Users\root\AppData\Local\Programs\Python\Python312\python.exe
DEBUG Running `python -V`
Python 3.12.10
DEBUG Command exited with code: 0
```


---

_Comment by @zanieb on 2025-08-24 14:40_

Ignore those Docker failures, we're looking into that.

How does this differ from https://github.com/astral-sh/uv/pull/13948?

---

_Comment by @sunfkny on 2025-08-24 14:47_

> Ignore those Docker failures, we're looking into that.
> 
> How does this differ from #13948?

`file_version.Patch` is not patch, it is `micro * 1000 + levelnum * 10 + serial`

It is actually calculated here:
https://github.com/python/cpython/blob/96b7a2eba423b42320f15fd4974740e3e930bb8b/PCbuild/python.props#L224-L230
and set here:
https://github.com/python/cpython/blob/96b7a2eba423b42320f15fd4974740e3e930bb8b/PCbuild/pyproject.props#L112

In the Windows resource files for the Python executable, the version is first defined in `python_ver_rc.h`:
`#define PYVERSION64 PY_MAJOR_VERSION, PY_MINOR_VERSION, FIELD3, PYTHON_API_VERSION`
https://github.com/python/cpython/blob/6fcac09401e336b25833dcef2610d498e73b27a1/PC/python_ver_rc.h#L34

Then it is used in `python_exe.rc` in `FILEVERSION`:
`FILEVERSION PYVERSION64`
https://github.com/python/cpython/blob/6fcac09401e336b25833dcef2610d498e73b27a1/PC/python_exe.rc#L24

The `FILEVERSION` statement is structured as:
`HIWORD(dw1), LOWORD(dw1), HIWORD(dw2), LOWORD(dw2)`
https://learn.microsoft.com/en-us/windows/win32/menurc/versioninfo-resource

Here, `file_version.Patch` corresponds to `HIWORD(dw2)`.

---

_@zanieb reviewed on 2025-08-24 15:05_

---

_Review comment by @zanieb on `crates/uv-python/src/pe_version.rs`:4 on 2025-08-24 15:05_

Maybe we can make this whole module cfg'd for windows like `microsoft_store ` is and drop the `try_extract_version_from_pe` for non-windows?

---

_@zanieb reviewed on 2025-08-24 15:06_

---

_Review comment by @zanieb on `crates/uv-python/src/pe_version.rs`:113 on 2025-08-24 15:06_

We should use `thiserror `for formatting of this error case and have the enum variant be specific to the release level

---

_@zanieb reviewed on 2025-08-24 15:10_

---

_Review comment by @zanieb on `crates/uv-python/src/pe_version.rs`:98 on 2025-08-24 15:10_

Can these `from` calls truncate? Do we need to document why they're safe?

---

_@sunfkny reviewed on 2025-08-24 15:34_

---

_Review comment by @sunfkny on `crates/uv-python/src/pe_version.rs`:98 on 2025-08-24 15:34_

Converting `u16` to `u64` is safe, since `Version::new` requires an iterator over items of type `R` where `R: Borrow<u64>`

---
