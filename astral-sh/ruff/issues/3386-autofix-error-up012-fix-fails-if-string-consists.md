```yaml
number: 3386
title: "[Autofix error] UP012 fix fails if string consists of multiple parts"
type: issue
state: closed
author: mthuurne
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-03-07T15:34:39Z
updated_at: 2023-03-08T10:22:39Z
url: https://github.com/astral-sh/ruff/issues/3386
synced_at: 2026-01-12T15:54:43Z
```

# [Autofix error] UP012 fix fails if string consists of multiple parts

---

_@mthuurne_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Code:
```py
print(
    "part 1 "
    "part 2".encode()
)
```
Note that the bug also occurs if both parts of the string are on a single line.

Command:
```
$ ruff check testcase.py --fix
```
Relevant settings:
```toml
extend-select = ["UP"]
```
Version:
```
ruff 0.0.254
```



---

_Comment by @charliermarsh on 2023-03-07 15:35_

Does it fix it erroneously, or does it just avoid fixing it at all?

---

_Label `question` added by @charliermarsh on 2023-03-07 15:35_

---

_Comment by @mthuurne on 2023-03-07 15:36_

It prints:
```
error: Autofix introduced a syntax error. Reverting all changes.
```

---

_Comment by @charliermarsh on 2023-03-07 15:36_

Thank you :)

---

_Label `question` removed by @charliermarsh on 2023-03-07 15:36_

---

_Label `bug` added by @charliermarsh on 2023-03-07 15:36_

---

_Label `autofix` added by @charliermarsh on 2023-03-07 15:36_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-07 19:45_

---

_Closed by @charliermarsh on 2023-03-07 23:33_

---

_Comment by @mthuurne on 2023-03-08 10:22_

Already fixed... even development is super fast :)

---
