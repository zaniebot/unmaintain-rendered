---
number: 12520
title: DOC501 throwing errors when using Sphinx style docstrings
type: issue
state: open
author: Eutropios
labels:
  - docstring
  - preview
assignees: []
created_at: 2024-07-26T02:09:43Z
updated_at: 2024-12-11T02:54:24Z
url: https://github.com/astral-sh/ruff/issues/12520
synced_at: 2026-01-07T13:12:15-06:00
---

# DOC501 throwing errors when using Sphinx style docstrings

---

_Issue opened by @Eutropios on 2024-07-26 02:09_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
DOC501, pydoclint, pydoclint 501, error in docstring, sphinx, sphinx docstring, rst, rst docstring
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
-->
```py
def foo(bar: int) -> float:
    """
    Minimal docstring.

    :param bar: Bar
    :raises ValueError: Foo bar.
    :return baz: Baz
    """
    if bar:
        raise ValueError
    return 0.1
```

Command used:
`ruff check --select DOC --preview .`

Ruff version:
Ruff v0.5.5

Description of bug:
Ruff throws a warning for DOC501 when the docstring already documents the thrown exceptions.

Output:
```bash
project/foo.py:150:15: DOC501 Raised exception `ValueError` missing from docstring
    |
148 |     """
149 |     if bar:
150 |         raise ValueError
    |               ^^^^^^^^^^ DOC501
151 |     return 0.1
```

---

_Label `bug` added by @charliermarsh on 2024-07-26 02:10_

---

_Label `docstring` added by @charliermarsh on 2024-07-26 02:10_

---

_Label `preview` added by @charliermarsh on 2024-07-26 02:10_

---

_Comment by @charliermarsh on 2024-07-26 02:11_

I believe this is a TODO per https://github.com/astral-sh/ruff/issues/12434.

---

_Label `bug` removed by @charliermarsh on 2024-07-26 02:11_

---

_Comment by @Eutropios on 2024-07-26 02:12_

Ahhh, my mistake! Thanks for the quick response. 😄 

---

_Referenced in [tox-dev/pipdeptree#397](../../tox-dev/pipdeptree/pulls/397.md) on 2024-07-29 20:14_

---

_Referenced in [meltano/sdk#2590](../../meltano/sdk/pulls/2590.md) on 2024-08-05 23:40_

---

_Referenced in [astral-sh/ruff#13286](../../astral-sh/ruff/pulls/13286.md) on 2024-09-11 20:10_

---

_Referenced in [astral-sh/ruff#14609](../../astral-sh/ruff/issues/14609.md) on 2024-11-26 13:34_

---

_Referenced in [tox-dev/tox-uv#210](../../tox-dev/tox-uv/pulls/210.md) on 2025-06-20 10:28_

---
