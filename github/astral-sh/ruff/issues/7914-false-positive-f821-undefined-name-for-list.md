---
number: 7914
title: "False positive F821 (Undefined name) for list comprehension in Annotated[]"
type: issue
state: closed
author: guillaume-alliander
labels: []
assignees: []
created_at: 2023-10-11T09:28:18Z
updated_at: 2023-10-11T13:33:17Z
url: https://github.com/astral-sh/ruff/issues/7914
synced_at: 2026-01-07T13:12:15-06:00
---

# False positive F821 (Undefined name) for list comprehension in Annotated[]

---

_Issue opened by @guillaume-alliander on 2023-10-11 09:28_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Without further ado, the snippet:

```python
from typing import Annotated

some_list = ["1", "2", "3"]
desc = f"Allowed values: {[p for p in some_list]}"

class WorkingModel:
    profiles: Annotated[
        list[str],
        desc,  # Ruff is happy
    ]

class FailingModel:
    profiles: Annotated[
        list[str],
        # Ruff complains about p being undefined
        f"Allowed values: {[p for p in some_list]}",
    ]
```

Ruff complains about the second `Annotated` parameter of `FailingModel`, with: `F821 Undefined name "p"`. I think it should not be an error, and creating the string outside annotated works fine, which is an inconsistency. 

For the record, this is a minimal example which might not mean much. The real Model extends `BaseModel` from `pydantic`, and the second part of `Annotated` uses `Query` from `Fastapi`, with an Enum instead of a list. But both versions show the same ruff error.

Ran with `ruff wtf.py --isolated`, ruff version 0.0.292 (and 0.0.291)

My pyproject.toml has this for ruff, which I think is not too relevant here:

```toml
[tool.ruff]
line-length = 120
# Group violations by containing file.
output-format = "grouped"
select = [
    # See https://docs.astral.sh/ruff/rules/
    "E", # Default, pyflake
    "F", # Default, pyflake
    "I", # isort
    "W", # warning
    "C90", # McCabe
    "PL", # Pylint (a subset thereof)
    "RUF", # RUF rules
    "UP", # upgrade python rules
    "B", # catch common bugs
    "A", # catch builtin shadowing
    "C4", # unnecessary things in/with comprehensions
    "DTZ", # datetime good practice
    "PIE", # unnecessary args/code for some well known code
    "PT", # pytest linting
    "SIM", # simplify code
    "ARG", # unused arguments
    "PTH", # use pathlib instead of os & co
    "PD", # pandas things
    "PERF", # performance things
    "N", # pep8
    "BLE", # catch too broad excpetion
]
[tool.ruff.pep8-naming]
# extend= use the default ignored names and add those
extend-ignore-names = [
    "G", # makes sense in the context of graph
    "mRID", "lineColor", # cim notation
    "create_*", # some functions are named after CIM nomenclatures, not python
    "N", # shortcut in stack
    ]
```



---

_Comment by @T-256 on 2023-10-11 09:36_

I believe it fixed in #7885

---

_Comment by @charliermarsh on 2023-10-11 13:20_

Yes, should be fixed on main -- thanks for the clear issue!

---

_Closed by @charliermarsh on 2023-10-11 13:20_

---

_Comment by @charliermarsh on 2023-10-11 13:20_

Funny that this came up twice in one week after being unnoticed for months :joy:

---

_Comment by @guillaume-alliander on 2023-10-11 13:33_

Weird indeed 😕 
Nice, thanks for the quick fix and the awesome tool! 

---
