```yaml
number: 411
title: Relative imports report as unresolved.
type: issue
state: closed
author: AfterThunk
labels:
  - imports
assignees: []
created_at: 2025-05-15T15:55:44Z
updated_at: 2025-05-16T13:22:10Z
url: https://github.com/astral-sh/ty/issues/411
synced_at: 2026-01-12T15:54:23Z
```

# Relative imports report as unresolved.

---

_@AfterThunk_

### Summary

I've looked through the existing issues and have found these, which are similar but don't seem exactly the same:
https://github.com/astral-sh/ty/issues/257
https://github.com/astral-sh/ty/issues/265
https://github.com/astral-sh/ty/issues/320
https://github.com/astral-sh/ty/issues/363
https://github.com/astral-sh/ty/issues/375

We have a project that has _no_ external dependencies or packages, for security reasons.
We use a standalone python executable (not installed).


The project is structured like this.
`ToolsAndServices` has a bunch of other languages and resources.
The `python` subdirectory is setup like a module, with an `__init__.py` and all of our source code (no subfolders)
```
/ToolsAndServices
    /python
        /__init__.py
        /build_helpers.py
        /command_line.py
        /main.py
        /utils.py
        /p4_utils.py
        /etc.py
    /non-python-stuff
```

We use simple batch and bash scripts to invoke it depending on platform:

```
@ECHO OFF

set PYTHONVER=Python-3.13.2
set "PYTHONHOME=%~dp0..\..\executables\win64\%PYTHONVER%"
set "PYTHONPATH=%~dp0..\.."
"%PYTHONHOME%\python.exe" -m python.main %*
```

I'm seeing a lot of errors like this:
```
error[unresolved-import]: Cannot resolve imported module `.command_line`
  --> D:\Builds\Depot\depot\ToolsAndServices\python\build_helpers.py:8:7
   |
 6 | from pathlib import Path
 7 |
 8 | from .command_line import CommandLineError
   |       ^^^^^^^^^^^^
 9 | from .utils import (get_workspace, get_command_path, load_env_file, get_env_checked)
10 | from .p4_utils import (P4ClientContext, P4ChangelistContext, delete_changelist,
   |
info: `unresolved-import` is enabled by default

error[unresolved-import]: Cannot resolve imported module `.utils`
  --> D:\Builds\Depot\depot\ToolsAndServices\python\build_helpers.py:9:7
   |
 8 | from .command_line import CommandLineError
 9 | from .utils import (get_workspace, get_command_path, load_env_file, get_env_checked)
   |       ^^^^^
10 | from .p4_utils import (P4ClientContext, P4ChangelistContext, delete_changelist,
   |
info: `unresolved-import` is enabled by default
```

It's worth noting that `ruff` doesn't seem to have a problem with getting type hints, and we didn't have any problems with this setup in `mypy`.

Because we have everything living in a single "module", we end up just using package relative imports.
https://docs.python.org/3/reference/import.html#package-relative-imports

This is how I'm invoking ty:
`ty.exe check /Path/To/ToolsAndServices/python`

### Version

ty 0.0.0-alpha.8 (0474b40e1 2025-05-09)

---

_Label `imports` added by @AlexWaygood on 2025-05-15 15:58_

---

_Comment by @carljm on 2025-05-16 02:42_

Can you try either running `ty` from the `/Path/To/ToolsAndServices` directory, or adding `--project /Path/To/ToolsAndServices` to your command line?

---

_Comment by @AfterThunk on 2025-05-16 13:22_

I'm not sure what's changed, but I can't seem to reproduce this today, even with the exact same commands I was using yesterday, when it was 100%.

---

_Closed by @AfterThunk on 2025-05-16 13:22_

---
