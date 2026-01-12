```yaml
number: 13260
title: Unexpected changes occur when fixing D208 and D209 on the same line
type: issue
state: closed
author: kubotty
labels:
  - bug
  - rule
  - docstring
  - help wanted
assignees: []
created_at: 2024-09-06T07:43:19Z
updated_at: 2024-10-03T14:47:52Z
url: https://github.com/astral-sh/ruff/issues/13260
synced_at: 2026-01-12T15:54:52Z
```

# Unexpected changes occur when fixing D208 and D209 on the same line

---

_@kubotty_

When ruff fixes both D208 (over-indentation) and D209 (new-line-after-last-paragraph) on the same line, it results in unexpected changes.
I share the minimum test case.

Target code:
```python
def test_func(a: int) -> None:
    """Minimum test case.

    Parameters
    ----------
    a : int
        A parameter."""
    pass
```

Settings in pyproject.toml:
```toml
[tool.ruff]
show-fixes = true

[tool.ruff.lint]
select = ["D"]

[tool.ruff.lint.pydocstyle]
convention = "numpy"
```

Run above command:
```sh
ruff check target.py --fix
```

Expected results:
```python
def test_func(a: int) -> None:
    """Minimum test case.

    Parameters
    ----------
    a : int
        A parameter.
    """
    pass
```

but, actual:
```python
def test_func(a: int) -> None:
    """Minimum test case.

    Parameters
    ----------
    a : int
    A parameter.
    """
    pass
```


---

_Comment by @MichaReiser on 2024-09-06 08:30_

Thanks. I think the issue here is that `D208` should not be raised because it also doesn't raise for:

```python
def test_func(a: int) -> None:
    """Minimum test case.

    Parameters
    ----------
    a : int
        A parameter.
    """
    pass
```

---

_Label `bug` added by @MichaReiser on 2024-09-06 08:30_

---

_Label `rule` added by @MichaReiser on 2024-09-06 08:30_

---

_Label `help wanted` added by @MichaReiser on 2024-09-06 08:30_

---

_Label `docstring` added by @AlexWaygood on 2024-09-06 08:53_

---

_Comment by @kubotty on 2024-09-11 05:21_

I added this sentence to pyproject.toml as a temporary solution in my project.

```toml
[tool.ruff.lint]
unfixable = [
    "D208",
]
```

I suggest treating D208 as "--unsafe-fixes".

---

_Comment by @ukyen8 on 2024-09-14 19:09_

Hi, I would like to work on this if no one has taken it.

---

_Assigned to @ukyen8 by @MichaReiser on 2024-09-15 06:55_

---

_Comment by @dhruvmanila on 2024-10-03 14:47_

@MichaReiser I'm closing this issue considering #13372 is merged but if that only solves a part of this issue, feel free to re-open.

---

_Closed by @dhruvmanila on 2024-10-03 14:47_

---
