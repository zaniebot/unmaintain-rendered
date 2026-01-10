```yaml
number: 2679
title: "PLE2502: False positive for escaped unicode names ``'\\N{...}'``"
type: issue
state: closed
author: AA-Turner
labels:
  - bug
assignees: []
created_at: 2023-02-09T03:21:12Z
updated_at: 2023-02-10T21:59:30Z
url: https://github.com/astral-sh/ruff/issues/2679
synced_at: 2026-01-10T11:09:45Z
```

# PLE2502: False positive for escaped unicode names ``'\N{...}'``

---

_Issue opened by @AA-Turner on 2023-02-09 03:21_

The new bidirectional unicode detector (#2589) doesn't seem to recognise ``\N{...}`` escapes (https://docs.python.org/3/reference/lexical_analysis.html#string-and-bytes-literals), see below:--

```powershell
(sphinx) PS I:\Development\sphinx> type bug.py
print('\N{RIGHT-TO-LEFT MARK}')
(sphinx) PS I:\Development\sphinx> pylint --version
pylint 2.16.1
astroid 2.14.1
Python 3.11.0 | packaged by conda-forge | (main, Oct 25 2022, 06:12:32) [MSC v.1929 64 bit (AMD64)]
(sphinx) PS I:\Development\sphinx> ruff -V                                   
ruff 0.0.244
(sphinx) PS I:\Development\sphinx> pylint --disable=all --enable E2502 bug.py

--------------------------------------------------------------------
Your code has been rated at 10.00/10 (previous run: 10.00/10, +0.00)

(sphinx) PS I:\Development\sphinx> ruff --isolated --select PLE2502 bug.py   
bug.py:1:7: PLE2502 Avoid using bidirectional unicode
Found 1 error.
(sphinx) PS I:\Development\sphinx>
```

Ruff version 0.0.244

Pylint does not raise the error (see part one of the reproducer).

A

---

_Comment by @AA-Turner on 2023-02-09 03:21_

Copying @colin99d as the author of the new check.

A

---

_Label `bug` added by @charliermarsh on 2023-02-09 03:23_

---

_Comment by @charliermarsh on 2023-02-09 03:23_

Thanks for filing.

---

_Closed by @charliermarsh on 2023-02-10 21:59_

---
