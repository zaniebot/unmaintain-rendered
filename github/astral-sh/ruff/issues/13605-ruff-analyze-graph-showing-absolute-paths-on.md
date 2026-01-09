---
number: 13605
title: Ruff analyze graph showing absolute paths on windows and no extra dependencies
type: issue
state: open
author: inoa-jboliveira
labels:
  - windows
  - analyze
assignees: []
created_at: 2024-10-02T12:36:26Z
updated_at: 2024-10-02T22:47:55Z
url: https://github.com/astral-sh/ruff/issues/13605
synced_at: 2026-01-07T13:12:15-06:00
---

# Ruff analyze graph showing absolute paths on windows and no extra dependencies

---

_Issue opened by @inoa-jboliveira on 2024-10-02 12:36_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Running `ruff analyze graph` on windows produce the following output where all paths are absolute and even use universal format (UNC maybe?) to display the files. E.g:

    "myapp\\foo\\bar\\__init__.py": [],
    "myapp\\foo\\bar\\admin.py": [
      "\\\\?\\C:\\Data\\myproject\\myapp\\foo\\bar\\models.py",
      "\\\\?\\C:\\Data\\myproject\\myapp\\foo\\baz\\admin.py",
    ]

I would expect the following:

    "myapp\\foo\\bar\\__init__.py": [],
    "myapp\\foo\\bar\\admin.py": [
      "myapp\\foo\\bar\\models.py",
      "myapp\\foo\\baz\\admin.py",
    ]

Also, **it does not show any external dependencie**s (ones in the virtual env). Does seem like a bug.

Finally, since this is not really a windows vs linux path thing as a dependency graph do not care about the OS, would it be possible to use posix paths on this output every time?

---
ruff 0.6.8
windows 11



---

_Renamed from "Ruff graph analyze showing absolute paths on windows " to "Ruff analyze graph showing absolute paths on windows " by @inoa-jboliveira on 2024-10-02 12:45_

---

_Renamed from "Ruff analyze graph showing absolute paths on windows " to "Ruff analyze graph showing absolute paths on windows and no extra dependencies" by @inoa-jboliveira on 2024-10-02 12:45_

---

_Label `windows` added by @AlexWaygood on 2024-10-02 14:00_

---

_Label `analyze` added by @AlexWaygood on 2024-10-02 14:00_

---

_Comment by @charliermarsh on 2024-10-02 22:32_

> Also, it does not show any external dependencies (ones in the virtual env). Does seem like a bug.

This isn't a bug -- it doesn't look at your environment at all right now. Doing so would require that you give us a way to discover the environment you care about. It's not wired up.

---

_Comment by @charliermarsh on 2024-10-02 22:34_

(The paths we should def fix.)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-02 22:47_

---
