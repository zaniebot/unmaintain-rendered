```yaml
number: 15065
title: "bug: using pydocstyle (D) does not enable D417"
type: issue
state: closed
author: BenjPetr
labels:
  - question
  - docstring
assignees: []
created_at: 2024-12-19T13:55:53Z
updated_at: 2025-01-25T10:54:00Z
url: https://github.com/astral-sh/ruff/issues/15065
synced_at: 2026-01-10T11:09:56Z
```

# bug: using pydocstyle (D) does not enable D417

---

_Issue opened by @BenjPetr on 2024-12-19 13:55_

### Description of the bug
selecting pydocstyle (D) does not enable D417
issue might be linked to using 'convention = "numpy"'

### To Reproduce

ruff.toml:
```

[lint]
select = [ "D"]

[lint.pydocstyle]
convention = "numpy"

```

example.py:
(docstring error see var2 -> var3)
```

'''Example module that contains a function with type hints.'''


def example(var1: int, var2: str) -> str:
    '''Concat variables.

    Parameters
    ----------
    var1 : int
        This is an integer argument
    var3 : str
        This is a string argument

    Returns
    -------
    str
        This is a string that contains the values of the two arguments
    '''
    return f"var1: {var1}, var2: {var2}"

```
```
ruff check
All checks passed!
```
(Expected a D417 error)

change ruff.toml:
```
[lint]
select = [ "D", "D417"]

[lint.pydocstyle]
convention = "numpy"

```
or
change ruff.toml:
```
[lint]
select = [ "D"]
```

```
ruff check
D417 Missing argument description in the docstring for `example`: `var2`
  |
4 | def example(var1: int, var2: str) -> str:
  |     ^^^^^^^ D417
5 |     """Concat variables.
  |
```
(Expected D417 Error showed up)

Also: should there not be an error for "var3" in the docstring?

### Expected behavior
Selecting a rule enables all stable rules of the set.

### Environment information
ruff 0.8.3
Python 3.10.15


---

_Label `question` added by @dylwil3 on 2024-12-19 20:12_

---

_Comment by @dylwil3 on 2024-12-19 20:12_

Yep - this rule is currently only supported for the google convention. Apologies!

From [the docs](https://docs.astral.sh/ruff/rules/undocumented-param/):

> This rule is enabled when using the google convention, and disabled when using the pep257 and numpy conventions.


---

_Label `docstring` added by @AlexWaygood on 2024-12-19 23:16_

---

_Comment by @BenjPetr on 2024-12-20 08:00_

It would be nice to get some level of information about this. Are there any other excluded? Are there any other Rules with this behavior? When I'm using one Rule I don't always want to second guess if it uses all the subrules.

Thank you for your time

---

_Comment by @MichaReiser on 2024-12-20 08:09_

That's understandable. The [FAQ section](https://docs.astral.sh/ruff/faq/#does-ruff-support-jupyter-notebooks) lists the enabled rules by convention 

---

_Closed by @MichaReiser on 2025-01-25 10:54_

---
