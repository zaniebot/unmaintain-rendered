```yaml
number: 14494
title: "Extend `docstring-missing-exception` to empty exception description (`DOC501`)"
type: issue
state: open
author: sbrugman
labels:
  - rule
  - docstring
assignees: []
created_at: 2024-11-20T16:35:07Z
updated_at: 2025-01-21T13:40:58Z
url: https://github.com/astral-sh/ruff/issues/14494
synced_at: 2026-01-12T15:54:53Z
```

# Extend `docstring-missing-exception` to empty exception description (`DOC501`)

---

_@sbrugman_

The rule that checks missing documentation for parameters considers empty descriptions as undocumented (`undocumented-param`, DOC501).
We have a similar rule for exceptions, which considers empty descriptions as documented (`docstring-missing-exception`, D417).

This example only errors for DOC501:
```python
def func1(arg1: str, arg2: int) -> int:
    """

    Args:
        arg1:
        arg2:

    Raises:
        ValueError:
    """
    try:
        return int(arg1) + arg2
    except:
        raise ValueError("msg")
```

```console
 ruff check --select D417,DOC501 --preview example.py
```

Proposal: modify D417 to report an error for empty exception descriptions.

Alternative solution: split `undocumented` and `empty/missing` into two separate rules. 

This behaviour is especially useful when docstring stubs are auto-generated (https://github.com/astral-sh/ruff/issues/14492)

---

_Label `rule` added by @AlexWaygood on 2024-11-20 17:54_

---

_Label `docstring` added by @AlexWaygood on 2024-11-20 17:54_

---

_Comment by @jsh9 on 2025-01-13 02:01_

Hi,

I'm the author of _pydoclint_.

Just FYI, in _pydoclint_ version 0.6.0 (the current latest version), this code example generates no violations:

```python
def func1(arg1: str, arg2: int) -> int:
    """

    Args:
        arg1:
        arg2:

    Returns:
        int:

    Raises:
        ValueError:
    """
    try:
        return int(arg1) + arg2
    except:
        raise ValueError("msg")
```
(It has a "Returns" section in the docstring)

Here are the pydoclint config options I used:
* `style`: `google`
* `argTypeHintsInDocstring`: `False`
* `skipCheckingRaises`: `False`

So this may have been a bug only in Ruff and not in _pydoclint_.

---

_Comment by @MichaReiser on 2025-01-13 07:59_

There's some precedence for empty docstring rules, e.g. https://docs.astral.sh/ruff/rules/empty-docstring-section/ and https://docs.astral.sh/ruff/rules/empty-docstring/

---

_Comment by @brendan-m-murphy on 2025-01-21 13:40_

Not really a bug, but if you omit the colon after `ValueError` and write something like

```
Raises:
    ValueError some description
```
then the error message is the same as if you didn't have a `Raises:` section at all. It would be helpful if it just said you need to add a colon.

---
