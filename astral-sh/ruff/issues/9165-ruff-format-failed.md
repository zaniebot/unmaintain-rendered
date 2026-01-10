---
number: 9165
title: "Ruff: Format failed ()"
type: issue
state: closed
author: james-berkheimer
labels:
  - bug
assignees: []
created_at: 2023-12-16T22:42:43Z
updated_at: 2023-12-17T00:35:23Z
url: https://github.com/astral-sh/ruff/issues/9165
synced_at: 2026-01-10T01:22:48Z
---

# Ruff: Format failed ()

---

_Issue opened by @james-berkheimer on 2023-12-16 22:42_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Pip installed Version: 0.1.7
VS Code extension version: v2023.56.0

When I am saving some code in VS Code I get the title error popping up.  In the "Problems" tab in VS Code, it looks like Ruff is scanning files in my `/usr/lib/python3.11/logging` directory.  I am still new to using Ruff.  Please let me know if you need further information to debug this issue.

![image](https://github.com/astral-sh/ruff/assets/5049386/a307cb6a-4e6d-4cf4-8022-16ff2d045e17)


---

_Comment by @charliermarsh on 2023-12-16 22:59_

@james-berkheimer - It looks like the issue here may be that you have a syntax error in `infrastructure.py`. But we should probably avoid raising the error popup on syntax errors, since they're common in the course of editing the file. Will fix!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-16 22:59_

---

_Label `bug` added by @charliermarsh on 2023-12-16 22:59_

---

_Comment by @james-berkheimer on 2023-12-16 23:04_

Yeah I usually will do a quick save when editing to see if it auto-formats for me.  But I got that popup.  What prompted me to create the issue was that it seems to be looking into my system python install.  Is that anything to be concerned with?

---

_Comment by @charliermarsh on 2023-12-16 23:29_

It's not a problem, just a little annoying that if you open those files, Ruff will show errors even though they're outside of your "control".

One solution -- explicitly ignore that directory:

```json
{
  "ruff.lint.args": ["--extend-exclude", "usr/lib"]
}
```

---

_Comment by @james-berkheimer on 2023-12-16 23:34_

Ah I see, I must have accidentally opened that __init__ at some point.  Thanks!

---

_Comment by @charliermarsh on 2023-12-17 00:35_

Should be fixed in the latest release (building now), thanks!

---

_Closed by @charliermarsh on 2023-12-17 00:35_

---
