---
number: 19705
title: "scripts:1:1: E902 The system cannot find the file specified"
type: issue
state: closed
author: alanmehio
labels: []
assignees: []
created_at: 2025-08-02T17:32:33Z
updated_at: 2025-08-03T07:15:52Z
url: https://github.com/astral-sh/ruff/issues/19705
synced_at: 2026-01-07T13:12:16-06:00
---

# scripts:1:1: E902 The system cannot find the file specified

---

_Issue opened by @alanmehio on 2025-08-02 17:32_

### Summary

python version : 3.13.5
OS: windows 
tox version : 4.28.3 
ruff version : 0.12.7

**$ tox -e ruff**
```
ruff: commands[0]> ruff check src tests scripts
scripts:1:1: E902 The system cannot find the file specified. (os error 2)
Found 1 error.
ruff: exit 1 (0.04 seconds) \...\....> ruff check src tests scripts pid=13696
  ruff: FAIL code 1 (0.06=setup[0.02]+cmd[0.04] seconds)
  evaluation failed :( (0.54 seconds)
(.venv)
```

Now the above is not clear but if we add verbose as below 
**$ tox -e ruff -v**
```
.. 
... 

scripts:1:1: E902 The system cannot find the file specified. (os error 2)
Found 1 error.
ruff: exit 1 (0.05 seconds) ..\....> ruff check src tests scripts pid=25480
  ruff: FAIL code 1 (0.08=setup[0.03]+cmd[0.05] seconds)
  evaluation failed :( (0.66 seconds)
(.venv)
```

###  WORKAROUND:
in the pyproject.toml file we can add ignore rule as below 

```toml
....
....
[tool.ruff.lint]
select = [
    "E",  # pycodestyle errors
    "W",  # pycodestyle warnings
    "F",  # pyflakes
    "I",  # isort
    "B",  # flake8-bugbear
    "C4",  # flake8-comprehensions
    "UP",  # pyupgrade
    "ARG001", # unused arguments in functions
]
ignore = [
    "E501",  # line too long, handled by black
    "B008",  # do not perform function calls in argument defaults
    "W191",  # indentation contains tabs
    "B904",  # Allow raising exceptions without from e, for HTTPException
    **"E902"  # This is added for workaround** 
```

### Version

 0.12.7

---

_Comment by @MichaReiser on 2025-08-02 21:02_

I'm not sure I understand what your question. But what I understand from your logs is that ruff raises `E902` because it can't file or directory named `scripts`. Does the directory `scripts` exist, and do you have read permissions? 


---

_Comment by @alanmehio on 2025-08-03 07:15_

I am closing this issue since the directory "scripts" is added now 

---

_Closed by @alanmehio on 2025-08-03 07:15_

---
