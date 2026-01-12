```yaml
number: 6091
title: Conflict of D407 and D417 or error parsing D417
type: issue
state: closed
author: Veritogen
labels:
  - docstring
assignees: []
created_at: 2023-07-26T10:13:15Z
updated_at: 2023-07-26T13:17:46Z
url: https://github.com/astral-sh/ruff/issues/6091
synced_at: 2026-01-12T15:54:45Z
```

# Conflict of D407 and D417 or error parsing D417

---

_@Veritogen_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
-->

I experienced a conflict between D417 saying that argument descriptions are missing when the function is defined as below. If I remove the line of dashes below `Args` the message isn't shown anymore. 
``` 
def some_func(self, foo:str="bar") -> str:
    """
    _summary_.

    Args:
    ----
        foo (str, optional): _description_. Defaults to "a".

    Returns:
    -------
        str: _description_
    """
    print(foo)
``` 

If the indentation of the arg below the dashed line is removed, D417 isn't linted anymore.
``` 
def some_func(self, foo:str="bar") -> str:
    """
    _summary_.

    Args:
    ----
    foo (str, optional): _description_. Defaults to "a".

    Returns:
    -------
        str: _description_
    """
    print(foo)
``` 

If I define the function as follows, there also is no message for missing argument descriptions (D417) except that that the dashed lines are missing below `Args` and `Returns` which will then be autofixed and lead to highlighting D417 again. 
```
def some_func(foo:str="bar") -> str:
    """
    _summary_.

    Args:
        foo (str, optional): _description_. Defaults to "bar".

    Returns:
        str: _description_
    """
    print(foo)
```


pyproject.toml: 
```
[tool.ruff]
# Enable pycodestyle (`E`) and Pyflakes (`F`) codes by default.
select = ["E", "F", "I", "DJ", "C90", "N", "W", "D", "ANN", "S", "FBT", "B", "C4", "DTZ", "ICN", "PT", "Q", "RET", "SIM", "TCH", "ARG", "TD", "PLR", "PLW", "TRY", "COM", "ISC", "PL"]
ignore = ["D211", "D212"]

# Allow autofix for all enabled rules (when `--fix`) is provided.
fixable = ["A", "B", "C", "D", "E", "F", "G", "I", "N", "Q", "S", "T", "W", "ANN", "ARG", "BLE", "COM", "DJ", "DTZ", "EM", "ERA", "EXE", "FBT", "ICN", "INP", "ISC", "NPY", "PD", "PGH", "PIE", "PL", "PT", "PTH", "PYI", "RET", "RSE", "RUF", "SIM", "SLF", "TCH", "TID", "TRY", "UP", "YTT"]
unfixable = ["D400", "D415", "RET502"]
]
```
Version: 0.0.269 and 0.0.280

I use the VSCode (version 1.80.0) with the ruff plugin with v2023.32.0. The messages are being shown 

---

_Comment by @charliermarsh on 2023-07-26 13:17_

I played around with this and it looks like you might be mixing docstring conventions. This lints as expected for missing-arguments (Google-style):

```python
def some_func(foo:str="bar", bar = 1) -> str:
    """
    _summary_.

    Args:    
        foo (str, optional): _description_. Defaults to "bar".

    Returns:
        str: _description_
    """
    print(foo)
```

As does (NumPy-style):

```python
def some_func(foo:str="bar", bar = 1) -> str:
    """
    _summary_.

    Args:
    ----
    foo (str, optional): _description_. Defaults to "bar".

    Returns:
    -------
    str: _description_
    """
    print(foo)
```

But indenting the arguments following an underlined section name doesn't fit either convention, and indeed `pydocstyle` flags `D417` in that case too. So I think this is roughly working as intended for now.

By the way: I'd recommend setting a convention, like:

```toml
[tool.ruff.pydocstyle]
convention = "numpy"
```

Or:

```toml
[tool.ruff.pydocstyle]
convention = "google"
```

Which will disable any rules that are incompatible with the convention (e.g., for Google, it'll turn off the underline-related rules).


---

_Closed by @charliermarsh on 2023-07-26 13:17_

---

_Label `docstring` added by @charliermarsh on 2023-07-26 13:17_

---
