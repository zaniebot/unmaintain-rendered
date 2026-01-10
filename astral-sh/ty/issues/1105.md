```yaml
number: 1105
title: "[panic] recursive type alias?"
type: issue
state: closed
author: jaens
labels: []
assignees: []
created_at: 2025-08-28T17:37:22Z
updated_at: 2025-08-28T17:46:23Z
url: https://github.com/astral-sh/ty/issues/1105
synced_at: 2026-01-10T02:06:24Z
```

# [panic] recursive type alias?

---

_Issue opened by @jaens on 2025-08-28 17:37_

Error message:
```
❯ uvx ty check _repro.py

WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                          error[panic]: Panicked at /root/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `_repro.py`: `infer_scope_types(Id(1001)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Version: 0.0.1-alpha.19
info: Args: ["ty", "check", "_repro.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: check_file_impl(Id(c00))
             at crates/ty_project/src/lib.rs:527


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

Source code:

```python
from pathlib import Path

type DirDict = dict[str, str | DirDict]


def path_files_to_dict(path: Path, ignored_names: set[str] | None = None) -> DirDict:
    """Read a directory and the contents of its files into a nested dictionary."""
    if ignored_names is None:
        ignored_names = set()
    return {
        item.name: path_files_to_dict(item, ignored_names) if item.is_dir(follow_symlinks=False) else item.read_text()
        for item in path.iterdir()
        if item.name not in ignored_names
    }
```


---

_Comment by @jaens on 2025-08-28 17:44_

Duplicate of https://github.com/astral-sh/ty/issues/256 I assume.

---

_Closed by @jaens on 2025-08-28 17:44_

---

_Comment by @carljm on 2025-08-28 17:45_

This particular issue (though not all of #256) is actually fixed in ty main branch, we just need to make a new release. It works in the playground (which is always updated to latest main branch): https://play.ty.dev/a4b8ccf2-2449-4b44-a709-499077dd0aec

---
