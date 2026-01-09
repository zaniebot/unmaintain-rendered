---
number: 12363
title: "`SIM102` false positive when using walrus operator"
type: issue
state: closed
author: adam-sikora
labels:
  - fixes
assignees: []
created_at: 2024-07-17T13:01:07Z
updated_at: 2024-07-17T14:07:04Z
url: https://github.com/astral-sh/ruff/issues/12363
synced_at: 2026-01-07T13:12:15-06:00
---

# `SIM102` false positive when using walrus operator

---

_Issue opened by @adam-sikora on 2024-07-17 13:01_

The following code snippet does not pass validation for rule `SIM102` (Use a single if statement instead of nested if statements)
```
foo = {"a": 1}
if bar := foo["a"]:
    if bar == 1:
        print(bar)
```
Check command used: `ruff check --select="SIM102" example.py --isolated` (ruff version: `0.5.2`)

However the if statements cannot be combined as the rule suggests as
```
foo = {"a": 1}
if bar := foo["a"] and bar == 1:
    print(bar)
```
would lead to: `NameError: name 'bar' is not defined`

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


---

_Comment by @charliermarsh on 2024-07-17 13:54_

I think it does work as:

```python
foo = {"a": 1}
if (bar := foo["a"]) and bar == 1:
    print(bar)
```


---

_Comment by @charliermarsh on 2024-07-17 13:55_

And `ruff check --fix` does handle this case properly, so I think this is working as intended!

---

_Closed by @charliermarsh on 2024-07-17 13:55_

---

_Label `fixes` added by @charliermarsh on 2024-07-17 13:56_

---

_Comment by @adam-sikora on 2024-07-17 14:07_

Yeah, my bad for forgetting the correct operator priority. Thanks for the clarification

---
