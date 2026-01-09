---
number: 7155
title: Rule B006 cause autofix error
type: issue
state: closed
author: qarmin
labels: []
assignees: []
created_at: 2023-09-05T14:41:52Z
updated_at: 2023-09-05T16:10:58Z
url: https://github.com/astral-sh/ruff/issues/7155
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule B006 cause autofix error

---

_Issue opened by @qarmin on 2023-09-05 14:41_

Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select B006 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
def readLinkagesByLineFile(linkages_file, 
                           column_mapping={
                           }):    
    """
    """                           
```

error
```
/home/rafal/test/tmp_folder/861_IDX_0_RAND_97224242746230905011214.py:2:43: B006 Do not use mutable data structures for argument defaults
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/861_IDX_0_RAND_97224242746230905011214.py`, the rule codes B006, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/12524933/python_compressed.zip)


---

_Comment by @zanieb on 2023-09-05 14:50_

This occurs if there is not a newline at the end of a file after a docstring followed by some whitespace

```
error: Autofix introduced a syntax error in `example.py` with rule codes B006: invalid syntax. Got unexpected token ':' at byte offset 145
---
def readLinkagesByLineFile(linkages_file, 
                           column_mapping=None):    
    """
    """         if column_mapping is None:
        column_mapping = {}

---
```

due to logic at https://github.com/astral-sh/ruff/blob/37d244d17833de852d7c7f34fdb65f90fed79878/crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs#L178-L184


---

_Referenced in [astral-sh/ruff#7160](../../astral-sh/ruff/pulls/7160.md) on 2023-09-05 15:01_

---

_Closed by @zanieb on 2023-09-05 16:10_

---
