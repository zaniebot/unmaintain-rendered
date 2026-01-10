```yaml
number: 5751
title: "feat: more rust in trampoline"
type: pull_request
state: closed
author: samypr100
labels: []
assignees: []
draft: true
base: main
head: trampoline-more-rust
created_at: 2024-08-03T14:43:58Z
updated_at: 2024-08-08T02:00:06Z
url: https://github.com/astral-sh/uv/pull/5751
synced_at: 2026-01-10T13:31:54Z
```

# feat: more rust in trampoline

---

_Pull request opened by @samypr100 on 2024-08-03 14:43_

## Summary

This is an experimental PR to replace more unsafe calls with more rust while still trying to keep the binary size small enough. These changes roughly increase the size of the trampolines to about 80kb~ due to usage of `Command` which brings a large chunk of `core::fmt`. This was a alternate PR to #5750.

The primary changes here include changes from #5750 (post merge)
* Switch to use rust path components for ease of path management
* Leverage `std::process::exit` for process exit and cleanup
* Use `std::io::Error::last_os_error` for IO Errors to remove `FormatMessage` complexity
* Use `std::env::current_exe` to get the current executable instead of `GetModuleFileNameA`

In addition, this PR also includes
* Switching to using `Command` instead of `CreateProcessA` and remove the need for CLI argument parsing
* Switching to use `std::env::args_os` instead of `GetCommandLineA`

## Test Plan

Existings tests / local tests.

---

_Comment by @samypr100 on 2024-08-03 14:48_

CC @konstin since you already pre-reviewed some of the changes

---

_Comment by @samypr100 on 2024-08-08 02:00_

Closing as #5750 was merged. We can revisit the removal of `CreateProcessA`, `GetCommandLineA` and `Win32_System_Environment` in the future and this can serve as an example if we want a working example.

---

_Closed by @samypr100 on 2024-08-08 02:00_

---
