```yaml
number: 5750
title: "feat: more rust in trampoline"
type: pull_request
state: merged
author: samypr100
labels: []
assignees: []
merged: true
base: main
head: trampoline-tad-more-rust
created_at: 2024-08-03T14:43:55Z
updated_at: 2024-08-08T02:01:03Z
url: https://github.com/astral-sh/uv/pull/5750
synced_at: 2026-01-10T13:31:54Z
```

# feat: more rust in trampoline

---

_Pull request opened by @samypr100 on 2024-08-03 14:43_

## Summary

This is an experimental PR to replace more unsafe calls with more rust while still trying to keep the binary size small enough. These changes roughly increase the size of the trampolines to about 40kb~. This is a alternate PR to https://github.com/astral-sh/uv/pull/5751.

The primary changes here include
* Switch to use rust path components for ease of path management
* Leverage `std::process::exit` for process exit and cleanup
* Use `std::io::Error::last_os_error` for IO Errors to remove `FormatMessage` complexity
* Use `std::env::current_exe` to get the current executable instead of `GetModuleFileNameA`

## Test Plan

Added one more existing test case to trampoline tests.
Still need to verify dunce::canonicalize is desired or not on find_python_exe.

---

_Comment by @samypr100 on 2024-08-03 14:48_

CC @konstin since you already pre-reviewed some of the changes

---

_Comment by @konstin on 2024-08-05 09:59_

On my machine the wheel goes from 11798922 bytes to 11827535 bytes, a 0.2% increase so totally fine size wise.



---

_@konstin approved on 2024-08-05 10:01_

---

_Marked ready for review by @samypr100 on 2024-08-06 23:24_

---

_Merged by @konstin on 2024-08-07 08:19_

---

_Closed by @konstin on 2024-08-07 08:19_

---

_Branch deleted on 2024-08-08 02:01_

---
