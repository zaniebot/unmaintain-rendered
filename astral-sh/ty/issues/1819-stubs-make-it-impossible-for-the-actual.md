```yaml
number: 1819
title: Stubs make it impossible for the actual implementation to resolve private items in other modules
type: issue
state: closed
author: hermeschen1116
labels: []
assignees: []
created_at: 2025-12-09T03:42:53Z
updated_at: 2025-12-09T18:01:20Z
url: https://github.com/astral-sh/ty/issues/1819
synced_at: 2026-01-12T15:54:25Z
```

# Stubs make it impossible for the actual implementation to resolve private items in other modules

---

_@hermeschen1116_

### Summary

I recently manually generated stub files for my package to solve another problem. But some internal private modules which is not included in stub files cannot be resolve after that.

Here is the diagnostics
```shell
2025-12-09 11:37:55.250751 DEBUG Version: 0.0.1-alpha.32 (84a188116 2025-12-05)
2025-12-09 11:37:55.254099 DEBUG Architecture: aarch64, OS: macos, case-sensitive: case-insensitive
2025-12-09 11:37:55.254131 DEBUG Searching for a project in '/Users/hermeschen/Repo/work/stream-server'
2025-12-09 11:37:55.254439 DEBUG Resolving requires-python constraint: `>=3.12`
2025-12-09 11:37:55.254455 DEBUG Resolved requires-python constraint to: 3.12
2025-12-09 11:37:55.25447 DEBUG Project without `tool.ty` section: '/Users/hermeschen/Repo/work/stream-server'
2025-12-09 11:37:55.254495 DEBUG Searching for a user-level configuration at `/Users/hermeschen/.config/ty/ty.toml`
2025-12-09 11:37:55.255226 INFO Defaulting to python-platform `darwin`
2025-12-09 11:37:55.255262 DEBUG Resolving `VIRTUAL_ENV` environment variable: /Users/hermeschen/Repo/work/stream-server/.venv
2025-12-09 11:37:55.255305 DEBUG Attempting to parse virtual environment metadata at '/Users/hermeschen/Repo/work/stream-server/.venv/pyvenv.cfg'
2025-12-09 11:37:55.255387 DEBUG Searching for site-packages directory in sys.prefix /Users/hermeschen/Repo/work/stream-server/.venv
2025-12-09 11:37:55.255453 DEBUG Resolved site-packages directories for this virtual environment are: ["/Users/hermeschen/Repo/work/stream-server/.venv/lib/python3.12/site-packages"]
2025-12-09 11:37:55.255513 DEBUG Searching for real stdlib directory in sys.prefix /Users/hermeschen/.local/share/uv/python/cpython-3.12.12-macos-aarch64-none
2025-12-09 11:37:55.255519 DEBUG Resolved real stdlib path for this virtual environment is: /Users/hermeschen/.local/share/uv/python/cpython-3.12.12-macos-aarch64-none/lib/python3.12
2025-12-09 11:37:55.255524 DEBUG Including `.` and `./src` in `environment.root` because a `./src` directory exists
2025-12-09 11:37:55.255553 DEBUG Adding first-party search path `/Users/hermeschen/Repo/work/stream-server/src`
2025-12-09 11:37:55.255569 DEBUG Adding first-party search path `/Users/hermeschen/Repo/work/stream-server`
2025-12-09 11:37:55.255572 DEBUG Using vendored stdlib
2025-12-09 11:37:55.255821 DEBUG Adding site-packages search path `/Users/hermeschen/Repo/work/stream-server/.venv/lib/python3.12/site-packages`
2025-12-09 11:37:55.255849 INFO Python version: Python 3.12, platform: darwin
2025-12-09 11:37:55.255897 DEBUG Adding new file root '/Users/hermeschen/Repo/work/stream-server/.venv/lib/python3.12/site-packages' of kind LibrarySearchPath
2025-12-09 11:37:55.256587 DEBUG Adding new file root '/Users/hermeschen/Repo/work/stream-server' of kind Project
2025-12-09 11:37:55.256709 DEBUG Setting included paths: 1
2025-12-09 11:37:55.256713 DEBUG Reloading files for project `stream-server`
2025-12-09 11:37:55.256743 DEBUG Starting main loop
2025-12-09 11:37:55.256751 DEBUG Waiting for next main loop message.
2025-12-09 11:37:55.256827 DEBUG Checking all files in project 'stream-server'
2025-12-09 11:37:55.260861 INFO Indexed 1 file(s) in 0.004s
2025-12-09 11:37:55.261447 DEBUG Checking file '/Users/hermeschen/Repo/work/stream-server/src/stream_server/internal/stream/Manager.py'
2025-12-09 11:37:55.270649 DEBUG Resolving dynamic module resolution paths
2025-12-09 11:37:55.291243 DEBUG Module `stream_server.internal.Typing._StreamInfo` not found in search paths
2025-12-09 11:37:55.291293 DEBUG Module `stream_server.internal.Typing._StreamInfo` not found while looking in parent dirs
2025-12-09 11:37:55.299574 DEBUG Module `stream_server.internal.Typing._StreamInfo` not found while looking in parent dirs
2025-12-09 11:37:55.30468 DEBUG Checking all files took 0.044s
error[unresolved-import]: Module `stream_server.internal.Typing` has no member `_StreamInfo`
  --> src/stream_server/internal/stream/Manager.py:13:43
   |
12 | from stream_server import BASE_LOGGER
13 | from stream_server.internal.Typing import _StreamInfo
   |                                           ^^^^^^^^^^^
14 | from stream_server.internal.stream.Streamer import Streamer
15 | from stream_server.internal.stream._GStreamer import PipelineState
   |
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic
2025-12-09 11:37:55.304835 DEBUG Exiting main loop
```

### Version

ty 0.0.1-alpha.32 (84a188116 2025-12-05)

---

_Comment by @Gankra on 2025-12-09 03:55_

How have you placed your stubs relative to the actual implementation?

If you generated a fully separate package you could potentially use a ["partial" py.typed](https://peps.python.org/pep-0561/#partial-stub-packages) to signal that there are missing modules in the package and to check the real package too.

---

_Comment by @hermeschen1116 on 2025-12-09 04:04_

> How have you placed your stubs relative to the actual implementation?

Yeah, like this
<img width="446" height="640" alt="Image" src="https://github.com/user-attachments/assets/d9490a9d-8703-4f9f-8048-c63663d5b40f" />

> If you generated a fully separate package you could potentially use a ["partial" py.typed](https://peps.python.org/pep-0561/#partial-stub-packages) to signal that there are missing modules in the package and to check the real package too.

And I just add "partial" with a new line in my py.typed, but it seems not work?


---

_Comment by @Gankra on 2025-12-09 04:11_

Oh I see, it's not the module failing to resolve, but rather an internal type that exists only in the impl but not the stub. Yes that's an unfortunate problem, I'm not sure if there's any great solutions for that.

---

_Comment by @hermeschen1116 on 2025-12-09 04:59_

Or, if I put the stub files in a special folder, would that work? I only have these files because I use nuitka to build my package.

---

_Comment by @AlexWaygood on 2025-12-09 13:46_

Related: https://github.com/astral-sh/ty/issues/1306

---

_Renamed from "Ty cannot resolve internal module if stub file is provide" to "Stubs make it impossible for the actual implementation to resolve cross-module private items" by @Gankra on 2025-12-09 15:27_

---

_Renamed from "Stubs make it impossible for the actual implementation to resolve cross-module private items" to "Stubs make it impossible for the actual implementation to resolve private items in other modules" by @Gankra on 2025-12-09 15:28_

---

_Comment by @carljm on 2025-12-09 17:58_

I'm not sure I see this as something that needs to be or should be fixed in ty? If you are going to include stubs inline in the project, then they are the source of truth for a type checker for that module. So if you are going to type-check your own project with those stubs included, then they need to include everything you use internally. IMO this is just how stub files work; I would be surprised if this works with any other type checker, and I don't see what ty can reasonably do to fix it.

If the stubs are just for end-users and not for type-checking your own project, then they should be distributed separately as a -stubs package, and not kept inline with your source code.

I think we should close this as not-planned.

---

_Closed by @carljm on 2025-12-09 18:01_

---
