```yaml
number: 1092
title: Windows launchers using posy trampolines
type: pull_request
state: merged
author: konstin
labels:
  - windows
assignees: []
merged: true
base: main
head: konsti/windows-launcher
created_at: 2024-01-25T13:33:31Z
updated_at: 2024-01-26T16:57:34Z
url: https://github.com/astral-sh/uv/pull/1092
synced_at: 2026-01-10T15:39:03Z
```

# Windows launchers using posy trampolines

---

_Pull request opened by @konstin on 2024-01-25 13:33_

## Background

In virtual environments, we want to install python programs as console commands, e.g. `black .` over `python -m black .`. They may be called [entrypoints](https://packaging.python.org/en/latest/specifications/entry-points/) or scripts. For entrypoints, we're given a module name and function to call in that module.

On Unix, we generate a minimal python script launcher. Text files are runnable on unix by adding a shebang at their top, e.g.

```python
#!/usr/bin/env python
```

will make the operating system run the file with the current python interpreter. A venv launcher for black in `/home/ferris/colorize/.venv` (module name: `black`, function to call: `patched_main`) would look like this:

```python
#!/home/ferris/colorize/.venv/bin/python
# -*- coding: utf-8 -*-
import re
import sys
from black import patched_main
if __name__ == "__main__":
    sys.argv[0] = re.sub(r"(-script\.pyw|\.exe)?$", "", sys.argv[0])
    sys.exit(patched_main())
```

On windows, this doesn't work, we can only rely on launching `.exe` files. 

## Summary

We use posy's rust implementation of a trampoline, which is based on distlib's c++ implementation. We pre-build a minimal exe and append the launcher script as stored zip archive behind it. The exe will look for the venv python interpreter next to it and use it to execute the appended script.

The changes in this PR make the `black` entrypoint work:

```powershell
cargo run -- venv .venv
cargo run -q -- pip install black
.\.venv\Scripts\black --version
```

Integration with our existing tests will be done in follow-up PRs.

## Implementation and Details

I've vendored the posy trampoline crate. It is a formatted, renamed and slightly changed for embedding version of https://github.com/njsmith/posy/pull/28.

The posy launchers are smaller than the distlib launchers, 16K vs 106K for black. Currently only `x86_64-pc-windows-msvc` is supported. The crate requires a nightly compiler for its no-std binary size tricks.

On windows, an application can be launched with a console or without (to create windows instead), which needs two different launchers. The gui launcher will subsequently use `pythonw.exe` while the console launcher uses `python.exe`.

---

_Label `windows` added by @konstin on 2024-01-25 13:33_

---

_@charliermarsh reviewed on 2024-01-25 17:36_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/lib.rs`:73 on 2024-01-25 17:36_

Can change the error name here?

---

_@charliermarsh reviewed on 2024-01-25 17:36_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/lib.rs`:71 on 2024-01-25 17:36_

"Unable to create Windows launch for {0} (only x64_64 is supported)"


---

_@charliermarsh approved on 2024-01-25 17:37_

I'm not reviewing the vendored code, but this looks great. Why does the trampoline crate require nightly? Can we remove that requirement?

---

_Merged by @konstin on 2024-01-26 13:54_

---

_Closed by @konstin on 2024-01-26 13:54_

---

_Branch deleted on 2024-01-26 13:54_

---

_Comment by @konstin on 2024-01-26 16:57_

> Why does the trampoline crate require nightly? Can we remove that requirement?

Building std and custom panic handler are required for the minimal binary sized. Tracked in #1126.

---
