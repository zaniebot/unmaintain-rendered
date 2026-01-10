```yaml
number: 2284
title: Use reparse points to detect Windows installer shims
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
created_at: 2024-03-07T18:40:07Z
updated_at: 2024-03-07T20:57:37Z
url: https://github.com/astral-sh/uv/pull/2284
synced_at: 2026-01-10T14:54:43Z
```

# Use reparse points to detect Windows installer shims

---

_Pull request opened by @charliermarsh on 2024-03-07 18:40_

## Summary

This PR enables use of the Windows Store Pythons even with `py` is not installed. Specifically, we need to ensure that the `python.exe` and `python3.exe` executables installed into the `C:\Users\crmar\AppData\Local\Microsoft\WindowsApp` directory _are_ used when they're not "App execution aliases" (which merely open the Windows Store, to help you install Python).

When `py` is installed, this isn't strictly necessary, since the "resolved" executables are discovered via `py`. These look like `C:\Users\crmar\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.11_qbs5n2kfra8p0\python.exe`.

Closes https://github.com/astral-sh/uv/issues/2264.

## Test Plan

- Removed all Python installations from my Windows machine.
- Uninstalled `py`.
- Enabled "App execution aliases".
- Verified that for both `cargo run venv --python python.exe` and `cargo run venv --python python3.exe`, `uv` exited with a failure that no Python could be found.
- Installed Python 3.10 via the Windows Store.
- Verified that the above commands succeeded without error.
- Verified that `cargo run venv --python python3.10.exe` _also_ succeeded.


---

_Review requested from @MichaReiser by @charliermarsh on 2024-03-07 18:41_

---

_Review requested from @konstin by @charliermarsh on 2024-03-07 18:41_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/python_query.rs`:549 on 2024-03-07 18:42_

Previously, this wouldn't work for one of the two shims. But adding the null terminator seems to fix it on my machine.

---

_@charliermarsh reviewed on 2024-03-07 18:42_

---

_Label `bug` added by @charliermarsh on 2024-03-07 18:45_

---

_Label `windows` added by @charliermarsh on 2024-03-07 18:45_

---

_@MichaReiser reviewed on 2024-03-07 20:34_

---

_Review comment by @MichaReiser on `crates/uv-interpreter/src/python_query.rs`:549 on 2024-03-07 20:34_

Adding the null termination seems correct. OsStr is not null terminated https://doc.rust-lang.org/std/ffi/struct.OsString.html

---

_Review comment by @MichaReiser on `crates/uv-interpreter/src/python_query.rs`:582 on 2024-03-07 20:36_

Nit: not a 100 sure but I think we could use mem::size_of here (or size_of_value) to avoid the * 2

---

_Review comment by @MichaReiser on `crates/uv-interpreter/src/python_query.rs`:594 on 2024-03-07 20:38_

Should this buf be sliced to bytes returned? It might not matter because it only tests startswidth but might still be good practice

---

_Review comment by @MichaReiser on `crates/uv-interpreter/src/python_query.rs`:510 on 2024-03-07 20:40_

Can we reuse the uv-fs function instead of reimplementing the check?

---

_Review comment by @MichaReiser on `crates/uv-fs/src/path.rs`:120 on 2024-03-07 20:43_

This can be simplified to using components.ends_with("Microsoft/WindowsApps") after having done the fuzzy python.exe check

---

_Merged by @charliermarsh on 2024-03-07 20:47_

---

_Closed by @charliermarsh on 2024-03-07 20:47_

---

_Branch deleted on 2024-03-07 20:47_

---

_Review comment by @MichaReiser on `crates/uv-interpreter/src/python_query.rs`:133 on 2024-03-07 20:48_

I don't know how to make code suggestions on ipad…

I would add #[cfg(windows)] to the if block instead of having an empty non-windows implementation in the windows module

---

_@MichaReiser approved on 2024-03-07 20:50_


Thanks for fixing this. I should have tested this workflow when improving windows support. And nice find with the null terminator

Have you considered upstreaming this back to Rye.

But what about https://twitter.com/charliermarsh/status/1765750403461689701


---

_Comment by @charliermarsh on 2024-03-07 20:55_

> But what about https://twitter.com/charliermarsh/status/1765750403461689701

This does NOT apply to Windows Store Python.

---

_@charliermarsh reviewed on 2024-03-07 20:57_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/python_query.rs`:510 on 2024-03-07 20:57_

They're actually different checks (sigh). The one in `uv-fs` needs to accept `python3.12.exe` and friends. The one here only accepts `python3.exe` and `python.exe`, since those are the only ones that get added as app redirects.

---

_@charliermarsh reviewed on 2024-03-07 20:57_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/python_query.rs`:133 on 2024-03-07 20:57_

Sigh, I originally did this but the cross-platform Clippy warnings were so brutal.

---
