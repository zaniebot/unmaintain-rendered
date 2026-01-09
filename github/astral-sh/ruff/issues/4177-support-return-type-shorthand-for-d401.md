---
number: 4177
title: Support return type shorthand for D401
type: issue
state: open
author: charliermarsh
labels:
  - docstring
assignees: []
created_at: 2023-05-02T06:16:01Z
updated_at: 2023-05-02T06:58:39Z
url: https://github.com/astral-sh/ruff/issues/4177
synced_at: 2026-01-07T13:12:14-06:00
---

# Support return type shorthand for D401

---

_Issue opened by @charliermarsh on 2023-05-02 06:16_

### Discussed in https://github.com/charliermarsh/ruff/discussions/4057

<div type='discussions-op-text'>

<sup>Originally posted by **ooliver1** April 21, 2023</sup>
I cannot find the name of this, if it is part of the numpy style, parsed from `ext.napoleon`, `ext.autodoc`, Sphinx or part of ReSructuredText itself, but an example is in the [ext.napoleon example](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_numpy.html)

```py
    @property
    def readonly_property(self):
        """str: Properties should be documented in their getter method."""
        return "readonly_property"

    @property
    def readwrite_property(self):
        """:obj:`list` of :obj:`str`: Properties with both a getter and setter
        should only be documented in their getter method.

        If the setter method contains notable behavior, it should be
        mentioned here.
        """
        return ["readwrite_property"]
```

In general:
```py
def func(...) -> T:
    """T: Summary"""
```

Currently, D401 ("First line of docstring should be in imperative mood:") does not detect non-imperative in the summary as it sees the first word as `str`, `` :class:`str` ``, etc.</div>

---

_Label `docstring` added by @charliermarsh on 2023-05-02 06:58_

---
