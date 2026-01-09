---
number: 12152
title: escape-sequence-in-docstring (D301) reports escaped docstrings within docstrings
type: issue
state: closed
author: tjkuson
labels:
  - bug
  - rule
  - docstring
  - help wanted
assignees: []
created_at: 2024-07-02T14:54:29Z
updated_at: 2024-07-18T22:36:15Z
url: https://github.com/astral-sh/ruff/issues/12152
synced_at: 2026-01-07T13:12:15-06:00
---

# escape-sequence-in-docstring (D301) reports escaped docstrings within docstrings

---

_Issue opened by @tjkuson on 2024-07-02 14:54_

Running

```sh
ruff check --isolated --select D301
```

on

```python
def foo():
    """
    This docstring contains another docstring.

        def bar():
            \"\"\"Here it is!\"\"\"
    """
```

returns an [escape-sequence-in-docstring (D301)](https://docs.astral.sh/ruff/rules/escape-sequence-in-docstring/) diagnostic, printing

```
example.py:2:5: D301 Use `r"""` if any backslashes in a docstring
  |
1 |   def foo():
2 |       """
  |  _____^
3 | |     This docstring contains another docstring.
4 | |
5 | |         def bar():
6 | |             \"\"\"Here it is!\"\"\"
7 | |     """
  | |_______^ D301
  |
  = help: Add `r` prefix
```

However, adding the `r` prefix and removing the backslashes is a syntax error:

```python
def foo():
    r"""
    This docstring contains another docstring.

        def bar():
            """Here it is!"""
    """
```

Docstrings in within docstrings can appear when a docstring contains a code snippet (which is how I discovered the issue). I would expect D301 not to be raised, as making the docstring raw doesn't work.

Ruff version 0.5.0

Search terms: D301, backslash, escape

Update: managed to reproduce with

```python
def foo():
    """
    This docstring contains another docstring.

        def bar():
            \"""Here it is!\"""
    """
```

as well.

---

_Label `bug` added by @AlexWaygood on 2024-07-02 20:25_

---

_Label `rule` added by @AlexWaygood on 2024-07-02 20:25_

---

_Label `docstring` added by @AlexWaygood on 2024-07-02 20:25_

---

_Label `help wanted` added by @MichaReiser on 2024-07-03 06:33_

---

_Comment by @ukyen8 on 2024-07-03 23:01_

Hi, I would like to work on this issue. Is anyone already working on it?

---

_Comment by @dhruvmanila on 2024-07-04 04:41_

@ukyen8 I don't think so, go for it!

---

_Assigned to @ukyen8 by @MichaReiser on 2024-07-04 05:23_

---

_Referenced in [astral-sh/ruff#12192](../../astral-sh/ruff/pulls/12192.md) on 2024-07-04 21:27_

---

_Comment by @charliermarsh on 2024-07-18 22:36_

Closed by https://github.com/astral-sh/ruff/pull/12192#pullrequestreview-2187016174. Thanks!

---

_Closed by @charliermarsh on 2024-07-18 22:36_

---
