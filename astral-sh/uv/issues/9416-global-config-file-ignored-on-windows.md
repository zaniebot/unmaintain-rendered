```yaml
number: 9416
title: Global config file ignored on Windows
type: issue
state: closed
author: markuspi
labels:
  - bug
  - windows
assignees: []
created_at: 2024-11-25T13:18:30Z
updated_at: 2024-11-28T16:45:19Z
url: https://github.com/astral-sh/uv/issues/9416
synced_at: 2026-01-12T15:59:49Z
```

# Global config file ignored on Windows

---

_@markuspi_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

UV does not read the configuration file in `%SYSTEMDRIVE%\ProgramData\uv\uv.toml` unless the working directory happens to be `%SYSTEMDRIVE%\`.

### Root Cause

- The `SYSTEMDRIVE` environment variable consists only of a drive letter followed by a colon. There is no trailing backslash. E.g. `C:` `D:`
- The final path is assembled by UV as follows: 
  ```rust
  let candidate = PathBuf::from(system_drive).join("ProgramData\\uv\\uv.toml");
  ```
- `candidate` now contains a path like this: `C:ProgramData\uv\uv.toml`
- Since there is no backslash between `C:` and `ProgramData`, this path is [specified](https://learn.microsoft.com/en-us/dotnet/standard/io/file-path-formats) to be 
  > A *relative* path from the current directory of the C: drive.

  rather than an absolute path. UV thus only recognizes the config file if the CWD is the root of the system drive.
- Inserting the missing backslash resolves the problem

### Steps to reproduce:

1. Create file `%SYSTEMDRIVE%\ProgramData\uv\uv.toml`. For demonstration purposes, insert an invalid option 

```toml
doesnotexist = true
```

2. Run UV from the drive root to observe parse error (as expected)

```cmd
cd %SYSTEMDRIVE%\
uv cache prune
```
`error: Failed to parse: C:ProgramData\uv\uv.toml`

3. Run UV from any other directory on the same drive. Observe that no parse error was thrown since the global config file was not loaded at all (bug)

```cmd
cd %SYSTEMDRIVE%\Users
uv cache prune
```
`Pruning cache at ...`

### Note

Be aware that windows keeps track of working directories _per system drive_. Changing to another drive such as `D:/foo` might not reproduce the bug

### Version Information

* UV version 0.5.4

---

_Label `bug` added by @zanieb on 2024-11-25 23:14_

---

_Label `windows` added by @zanieb on 2024-11-25 23:14_

---

_Comment by @zanieb on 2024-11-25 23:16_

Are you sure "candidate now contains a path like this: C:ProgramData\uv\uv.toml" is true? `.join` should use the system separator to join the paths?

---

_Comment by @zanieb on 2024-11-25 23:18_

It looks like this is intentionally omitted, interesting

https://doc.rust-lang.org/src/std/path.rs.html#1288-1289

---

_Comment by @markuspi on 2024-11-26 08:32_

Yeah Windows Paths are quite a mess 😅
At least Rust's behavior of `.join` is in line with Python and Java implementations

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-28 00:01_

---

_Closed by @charliermarsh on 2024-11-28 00:38_

---

_Closed by @charliermarsh on 2024-11-28 00:38_

---

_Comment by @markuspi on 2024-11-28 16:45_

Thanks!

---
