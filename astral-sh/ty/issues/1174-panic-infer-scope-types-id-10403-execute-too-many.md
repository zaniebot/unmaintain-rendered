```yaml
number: 1174
title: "[panic] `infer_scope_types(Id(10403)): execute: too many cycle iterations`"
type: issue
state: closed
author: RandomBrainCode
labels: []
assignees: []
created_at: 2025-09-11T21:57:56Z
updated_at: 2025-09-11T22:17:30Z
url: https://github.com/astral-sh/ty/issues/1174
synced_at: 2026-01-12T15:54:24Z
```

# [panic] `infer_scope_types(Id(10403)): execute: too many cycle iterations`

---

_@RandomBrainCode_

 uv run ty check
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 42/42 files                                               error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/Users/<REDACTED>/projects/<REDACTED>/db/database.py`: `infer_scope_types(Id(10403)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: 0.0.1-alpha.19 (e9cb838b3 2025-08-19)
info: Args: ["ty", "check"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: check_file_impl(Id(2c00))
             at crates/ty_project/src/lib.rs:527


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

---

_Comment by @RandomBrainCode on 2025-09-11 22:04_

Error appears to be limited to a single file in my project:
```shell
uv run ty check db/database.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                 error[panic]: Panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25
```

I test ran directly on some other files in the project and they all passed successfully:

```shell
uv run ty check db/models/types.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                 All checks passed!

uv run ty check app/main.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                 All checks passed!

uv run ty check app/dependencies.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                 All checks passed!

```

I can't provide the contents of `db/database.py` publicly but if there is something specific you would like me to check for, please let me know and I would be happy to check it out.

---

_Comment by @RandomBrainCode on 2025-09-11 22:09_

Something else of note - I am also running `ty server` with LSP in PyCharm and it not throwing any error's when checking `db/database.py`, so based on this, I ran the following with the global install and it worked ok:

```shell
ty check db/database.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                 All checks passed!
```



---

_Comment by @RandomBrainCode on 2025-09-11 22:12_

```shell
╰─ uv run ty version
ty 0.0.1-alpha.19 (e9cb838b3 2025-08-19)


╰─ ty version
ty 0.0.1-alpha.20 (f41f00af1 2025-09-03)
```

---

_Comment by @RandomBrainCode on 2025-09-11 22:17_

Updated project requirement for ty to `"ty>=0.0.1a20"` and issue is now resolved:

```shell
╰─ uv sync
Resolved 102 packages in 3.34s
      Built <REDACTED> @ file:///Users/<REDACTED>/projects/<REDACTED>
Prepared 2 packages in 1.03s
Uninstalled 2 packages in 8ms
Installed 2 packages in 4ms
 ~ <REDACTED>==1.6.0.0 (from file:///Users//<REDACTED>/projects/<REDACTED>)
 - ty==0.0.1a19
 + ty==0.0.1a20

╰─ uv run ty check
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 42/42 files                                               All checks passed!
```

---

_Closed by @RandomBrainCode on 2025-09-11 22:17_

---
