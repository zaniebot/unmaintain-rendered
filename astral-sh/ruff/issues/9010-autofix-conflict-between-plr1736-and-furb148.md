---
number: 9010
title: "Autofix conflict between `PLR1736` and `FURB148`"
type: issue
state: closed
author: danparizher
labels:
  - bug
assignees: []
created_at: 2023-12-05T15:45:09Z
updated_at: 2023-12-05T20:25:29Z
url: https://github.com/astral-sh/ruff/issues/9010
synced_at: 2026-01-10T01:22:48Z
---

# Autofix conflict between `PLR1736` and `FURB148`

---

_Issue opened by @danparizher on 2023-12-05 15:45_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
The following autofix introduces a NameError when both `PLR1736` and `FURB148` are selected.

Before:
```py
letters = ["a", "b", "c"]

for index, letter in enumerate(letters):
    print(letters[index])
```
After:
```py
letters = ["a", "b", "c"]

for index in range(len(letters)):
    print(letter)
```
ruff 0.1.7

---

_Label `bug` added by @charliermarsh on 2023-12-05 16:01_

---

_Comment by @charliermarsh on 2023-12-05 16:01_

👍 We need to ensure that these fixes conflict so that they don't overwrite one another.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-05 19:51_

---

_Referenced in [astral-sh/ruff#9012](../../astral-sh/ruff/pulls/9012.md) on 2023-12-05 20:08_

---

_Closed by @charliermarsh on 2023-12-05 20:25_

---
