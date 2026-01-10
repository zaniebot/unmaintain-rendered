---
number: 13477
title: "[Feature Request] Add a rule that warns about assigning None"
type: issue
state: open
author: serjflint
labels:
  - rule
  - type-inference
  - needs-decision
assignees: []
created_at: 2024-09-23T06:36:56Z
updated_at: 2024-09-23T08:54:12Z
url: https://github.com/astral-sh/ruff/issues/13477
synced_at: 2026-01-10T01:22:53Z
---

# [Feature Request] Add a rule that warns about assigning None

---

_Issue opened by @serjflint on 2024-09-23 06:36_

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
Hi! I am switching linters in my codebase from flake8+pylint to ruff. 

I found an interesting [bug](https://github.com/python/mypy/issues/17803) in the code. People use `var = None` as a placeholder for values before a loop or a condition. The problem is that mypy considers that as `var: None = None`. 

Can ruff warn about that misuse and propose a change `var: SomeType | None = None`?

---

_Label `rule` added by @MichaReiser on 2024-09-23 08:54_

---

_Label `type-inference` added by @MichaReiser on 2024-09-23 08:54_

---

_Label `needs-decision` added by @MichaReiser on 2024-09-23 08:54_

---

_Comment by @MichaReiser on 2024-09-23 08:54_

Welcome to the ruff users :)

Because you're mentioning pylint and flake8: Do you know if pylint or flake8 have a rule that captures this pattern?

To clarify my understanding. You're referring to code like this (but it applies to all kinds of loops):

```python
var = None

for x in range(1, 10):
    var = x


print(x)
```

But it doesn't apply if the for loop has an `orelse` branch where the variable is assigned unconditionally (both in the for body and the orelse).


I'm a bit hesitant of adding such a rule because it assumes specific mypy behavior. We're in the process of building a type checker and our type checker would correctly infer the `var` type as `int | None` if it is unannotated. I also suspect that implementing said rules requires implementing something close to a type checker to have a low false positive rate. 

---
