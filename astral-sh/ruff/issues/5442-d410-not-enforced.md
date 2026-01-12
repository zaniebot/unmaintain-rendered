```yaml
number: 5442
title: D410 not enforced
type: issue
state: closed
author: erwann-met
labels:
  - bug
  - docstring
  - help wanted
assignees: []
created_at: 2023-06-29T15:03:21Z
updated_at: 2023-07-03T00:44:45Z
url: https://github.com/astral-sh/ruff/issues/5442
synced_at: 2026-01-12T15:54:45Z
```

# D410 not enforced

---

_@erwann-met_

Code to reproduce:
![image](https://github.com/astral-sh/ruff/assets/113345860/8e3a939b-3d78-4eab-904e-a5b7f2ee5d5a)

Running ruff v 0.0.275:
![image](https://github.com/astral-sh/ruff/assets/113345860/75f87b8a-b3b3-426d-8b63-1c449b77ce22)

D400 is well detected, proving that ruff indeed checked the file.
However `D410 no-blank-line-after-section` goes undetected (there should be a blank line before the `Returns` section.

This is an issue when trying to use `pydoclint` alongside `ruff` (to replace the annoyingly slow f`lake8` - `darglint` combo) as the error messages of `pydoclint` become unclear when this happens.

Here is the content of my `pyproject.toml` file:
```
[tool.ruff]
line-length = 120
select = ["D", "E", "F", "I", "W"]

ignore = [ # Set of rules tested on mts-api-python
    "D102", # Missing docstring in public method
    "D107", # Missing docstring in __init__
    "D203", # 1 blank line required before class docstring, conflicts with D211 no blank line before class docstring
    "D205", # 1 blank line required between summary line and description, very strict
    "D212", "D213", # Summary should start at the first or second line, both are ignored by default
    "D404", # First word of the docstring should not be "This"
]

[tool.ruff.per-file-ignores]
"__init__.py" = ["D104"] # ignore no docstrings in __init__.py
"tests/*" = ["D"] # ignore all docstrings related linting in tests
"*.ipynb" = ["D"] # ignore all docstrings related linting in notebooks
```


---

_Renamed from "D410 doesn't seem to be enforced" to "D410 not enforced" by @erwann-met on 2023-06-29 15:04_

---

_Label `bug` added by @charliermarsh on 2023-06-29 15:05_

---

_Label `docstring` added by @charliermarsh on 2023-06-29 15:05_

---

_Comment by @charliermarsh on 2023-06-29 15:05_

Looks like a bug, but need to check whether pydocstyle flags this.

---

_Label `help wanted` added by @charliermarsh on 2023-06-30 02:38_

---

_Comment by @charliermarsh on 2023-07-02 23:47_

pydocstyle doesn't catch this either. I think part of the problem is that we're not recognizing it as two separate sections.

---

_Closed by @charliermarsh on 2023-07-03 00:29_

---

_Comment by @charliermarsh on 2023-07-03 00:44_

Fixed in the next release, thanks.

---
