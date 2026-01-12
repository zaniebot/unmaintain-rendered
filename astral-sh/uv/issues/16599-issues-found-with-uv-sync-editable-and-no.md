```yaml
number: 16599
title: Issues Found with uv sync --editable and --no-editable Flags
type: issue
state: closed
author: DhavalGojiya
labels:
  - bug
assignees: []
created_at: 2025-11-05T12:28:44Z
updated_at: 2025-11-05T17:31:30Z
url: https://github.com/astral-sh/uv/issues/16599
synced_at: 2026-01-12T16:02:34Z
```

# Issues Found with uv sync --editable and --no-editable Flags

---

_@DhavalGojiya_

### Summary

### **Issue 1 - Missing documentation for `--editable` flag**

When running:

```bash
uv sync --editable
```

Output:

```
Resolved 69 packages in 1ms
Audited 46 packages in 0.74ms
```

The `--editable` flag works as expected (and is actually the default behavior of `uv sync` that install my project as editable mode),
but this flag is **not described** in either of the following help commands:

```bash
uv sync -h
uv help sync
```

 **Expected behavior:**
The `--editable` flag should be documented under `uv sync -h` and `uv help sync`,
indicating that it installs the project in editable mode (which is also the default).

---

### **Issue 2 - Conflicting flags: `--editable` and `--no-editable`**

When running:

```bash
uv sync --editable --no-editable
```

Output:

```
Resolved 69 packages in 1ms
Built fakeredis @ file:///home/dhaval/Open-Source/fakeredis-py
Prepared 1 package in 1.23s
Uninstalled 1 package in 28ms
Installed 1 package in 1ms
 ~ fakeredis==2.32.1 (from file:///home/dhaval/Open-Source/fakeredis-py)
```

Here, I’m explicitly asking `uv` to install the project as **both editable and non-editable** Mode,
which doesn’t make sense logically.

**Actual behavior:**
`uv` silently accepts both flags, and the **last defined flag wins** (in this case, `--no-editable`).

**Expected behavior:**
`uv` should **raise an error** if both `--editable` and `--no-editable` are provided together.

### Platform

Linux 6.8.0-45-generic x86_64 GNU/Linux

### Version

uv 0.9.7

### Python version

Python 3.12.6

---

_Label `bug` added by @DhavalGojiya on 2025-11-05 12:28_

---

_Comment by @charliermarsh on 2025-11-05 17:04_

Unfortunately both of these are intentional: to save space on the CLI, we only show the non-default option (between `--editable` and `--no-editable`, but also for other options); and we allow both `--editable` and `--no-editable` because the last-defined flag wins, and this is also intentional and not uncommon on CLIs.


---

_Comment by @DhavalGojiya on 2025-11-05 17:29_

> Unfortunately both of these are intentional: to save space on the CLI, we only show the non-default option (between `--editable` and `--no-editable`, but also for other options); and we allow both `--editable` and `--no-editable` because the last-defined flag wins, and this is also intentional and not uncommon on CLIs.

Oh, thank you @charliermarsh
I didn't know that uv doesn't show non-default arguments in the CLI help.
Completely understand your point.

---

_Comment by @DhavalGojiya on 2025-11-05 17:31_

We can close this issue.
Seems like everything is fine.

---

_Closed by @DhavalGojiya on 2025-11-05 17:31_

---
