---
number: 5975
title: "`Q001` incorrectly treats attribute docstrings as plain multiline strings"
type: issue
state: open
author: vpoverennov
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-07-22T12:08:26Z
updated_at: 2024-03-11T22:07:37Z
url: https://github.com/astral-sh/ruff/issues/5975
synced_at: 2026-01-07T13:12:15-06:00
---

# `Q001` incorrectly treats attribute docstrings as plain multiline strings

---

_Issue opened by @vpoverennov on 2023-07-22 12:08_

[PEP-0257](https://peps.python.org/pep-0257/) defines attribute docstrings

> String literals occurring elsewhere in Python code may also act as documentation. They are not recognized by the Python bytecode compiler and are not accessible as runtime object attributes (i.e. not assigned to `__doc__`), but two types of extra docstrings may be extracted by software tools:

String literals occurring immediately after a simple assignment at the top level of a module, class, or `__init__` method are called “attribute docstrings”.

```
class Test:
    """
    class docstring
    """

    x = 42
    """attribute docstring"""

    def __init__(self):
        """method docstring"""
        pass
```

with the following config

```
select = [
 "Q"
]

[flake8-quotes]
inline-quotes = "single"
multiline-quotes = "single"
docstring-quotes = "double"
```

results in 
```
# ruff main.py
main.py:7:5: Q001 [*] Double quote multiline found but single quotes preferred
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

Ruff follows the behavior of flake8-quotes here, but I still think this might be an issue which was never discovered in flake8
```
# flake8 main.py
main.py:7:5: Q001 Double quote multiline found but single quotes preferred
```

here's a [repo ](https://github.com/vpoverennov/ruff_quotes_repro) with the full setup
I'm using the latest ruff 0.0.280
Thank you


---

_Renamed from "attribute docstrings are recognised as multiline strings instead by Q001" to "Q001 incorrectly treats attribute docstrings as plain multiline strings" by @vpoverennov on 2023-07-22 12:10_

---

_Renamed from "Q001 incorrectly treats attribute docstrings as plain multiline strings" to "`Q001` incorrectly treats attribute docstrings as plain multiline strings" by @vpoverennov on 2023-07-22 12:11_

---

_Comment by @charliermarsh on 2023-07-22 14:13_

Thanks for the clear issue. A little tricky because AFAIK attribute docstrings were explicitly rejected in [PEP 224](https://peps.python.org/pep-0224/), so it's arguably incorrect to treat them as docstrings for the purpose of this rule, but I know they're used in practice in a variety of places and projects.

---

_Label `needs-decision` added by @charliermarsh on 2023-07-22 14:13_

---

_Label `rule` added by @charliermarsh on 2023-07-22 14:13_

---

_Comment by @vpoverennov on 2023-07-23 14:31_

I'm not familiar with the history of the mentioned PEPs, I've only recently learned about attribute docstrings myself. Pycharm and VSCode support them though.

[PEP-0224](https://peps.python.org/pep-0224/) and [PEP-0258](https://peps.python.org/pep-0258/#attribute-docstrings) are both rejected but seem to be talking about the runtime implementation.
[PEP-0257](https://peps.python.org/pep-0257/) is active and discusses only semantics of the attribute docstrings

Here's what I found on this topic:
[a dicsussion in vscode-python](https://github.com/microsoft/vscode-python/discussions/16736)
[SO discussion](https://stackoverflow.com/a/9558703) mentions that [Sphinx supports them as well](https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html#directive-autoattribute)

---

_Comment by @tibdex on 2024-03-11 22:07_

Related to #10347.

---
