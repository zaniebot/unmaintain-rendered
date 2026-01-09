---
number: 15165
title: "uvx can't find executables on Windows when the package name contains a \".\""
type: issue
state: closed
author: crh23
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-08-08T13:36:14Z
updated_at: 2025-08-15T21:45:00Z
url: https://github.com/astral-sh/uv/issues/15165
synced_at: 2026-01-07T13:12:19-06:00
---

# uvx can't find executables on Windows when the package name contains a "."

---

_Issue opened by @crh23 on 2025-08-08 13:36_

### Summary

Running (for example) `uvx awslabs.cdk-mcp-server@latest` on Windows will produce the error
```
error: Failed to spawn: `awslabs.cdk-mcp-server`
  Caused by: program not found
```

This can be worked around by running `uvx --from awslabs.cdk-mcp-server@latest awslabs.cdk-mcp-server.exe`, indicating that `uv` is not correctly finding the Windows executable.

I believe this is due to the extension guessing implementation found in https://github.com/astral-sh/uv/blob/57df0146e2850cc1746160357d448cf29e4829f9/crates/uv-shell/src/runnable.rs#L73-L100
since
``` 
script_path.with_extension(script_type.to_extension())
```
will replace the "extension" found after the "." of the original script name, so instead of searching for `awslabs.cdk-mcp-server.exe` it will search for `awslabs.exe` (see [the examples for with_extension](https://doc.rust-lang.org/std/path/struct.Path.html#method.with_extension)).



### Platform

Windows 11 x86_64

### Version

uv 0.8.0 (0b2357294 2025-07-17)

### Python version

Python 3.12.1

---

_Label `bug` added by @crh23 on 2025-08-08 13:36_

---

_Comment by @crh23 on 2025-08-08 13:37_

I see there's an experimental [with_added_extension](https://doc.rust-lang.org/std/path/struct.Path.html#method.with_added_extension) API, which could possibly be more suitable

---

_Referenced in [awslabs/mcp#1046](../../awslabs/mcp/issues/1046.md) on 2025-08-08 15:56_

---

_Comment by @crh23 on 2025-08-08 16:20_

When I run against https://github.com/astral-sh/uv/commit/82212bb439243acf4178c517b3797ad9b1dcf485 (the commit before https://github.com/astral-sh/uv/pull/12079/commits/0c8dd60018625410a5f0cbdb513713a4c3b5f279, which added `runnable.rs`) a more useful error is produced, so there is at least a mild regression here.
```
Installed 40 packages in 4.29s
The executable `awslabs.cdk-mcp-server` was not found.
warning: An executable named `awslabs.cdk-mcp-server` is not provided by package `awslabs-cdk-mcp-server`.
The following executables are provided by `awslabs-cdk-mcp-server`:
- awslabs.cdk-mcp-server.exe
Consider using `uvx --from awslabs-cdk-mcp-server <EXECUTABLE_NAME>` instead.
error: process didn't exit successfully: `target\debug\uvx.exe awslabs.cdk-mcp-server@latest` (exit code: 1)
```

---

_Comment by @zanieb on 2025-08-08 17:48_

Thanks for the details!

---

_Label `help wanted` added by @zanieb on 2025-08-08 17:48_

---

_Comment by @fallbackerik on 2025-08-08 18:08_

I think, to answer this question we need to determine whether `script_path.with_extension()` is working correctly. I'm a little curious if it's actually [this method here](https://doc.rust-lang.org/std/path/struct.PathBuf.html#method.with_extension).

Side note: if `script_path` is a Path or [PathBuf, there's an `extension()` method that correctly determines the extension even if there are multiple dots](https://doc.rust-lang.org/std/path/struct.PathBuf.html#method.extension).

---

_Referenced in [astral-sh/uv#15300](../../astral-sh/uv/pulls/15300.md) on 2025-08-15 10:07_

---

_Closed by @zanieb on 2025-08-15 21:45_

---

_Referenced in [awslabs/mcp#273](../../awslabs/mcp/issues/273.md) on 2025-08-20 09:12_

---

_Referenced in [awslabs/mcp#1114](../../awslabs/mcp/pulls/1114.md) on 2025-08-21 09:13_

---

_Referenced in [awslabs/mcp#1164](../../awslabs/mcp/issues/1164.md) on 2025-08-22 11:38_

---
