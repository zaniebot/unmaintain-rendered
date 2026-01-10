---
number: 9641
title: "Request: Lint rule to enforce ordering of docstring sections"
type: issue
state: open
author: stinodego
labels:
  - rule
  - docstring
  - accepted
assignees: []
created_at: 2024-01-25T13:53:59Z
updated_at: 2025-04-04T13:45:48Z
url: https://github.com/astral-sh/ruff/issues/9641
synced_at: 2026-01-10T01:22:49Z
---

# Request: Lint rule to enforce ordering of docstring sections

---

_Issue opened by @stinodego on 2024-01-25 13:53_

Here I come again with docstring requests :sweat_smile: 

Our project follows the NumPy docstring conventions outlined [here](https://numpydoc.readthedocs.io/en/latest/format.html).

One extremely common mistake our contributors make is the order in which docstring sections appear. Often I'll have to correct docstrings where people put the `Returns` section after the `Examples` section, or the `Notes` section before the `Warnings` section.

Could ruff autodetect/autofix this?

**Before**
```python
def cool_function() -> int:
    """
    Description.

    Notes
    -----
    Important edge case.

    Warnings
    --------
    Be aware of this requirement!

    Returns
    -------
    int
    """
```

**After**
```python
def cool_function() -> int:
    """
    Description.

    Returns
    -------
    int

    Warnings
    --------
    Be aware of this requirement!

    Notes
    -----
    Important edge case.
    """
```

---

_Comment by @charliermarsh on 2024-01-26 04:00_

Yeah, we should definitely lint for this given that it's part of the docstring spec.

Since pydocstyle has been deprecated, I would be comfortable adding this to the `D` rules directly. We could enable it for the NumPy convention (but disable it for the Google and PEP 257 conventions).

---

_Label `rule` added by @charliermarsh on 2024-01-26 04:00_

---

_Label `docstring` added by @charliermarsh on 2024-01-26 04:00_

---

_Label `accepted` added by @charliermarsh on 2024-01-26 04:00_

---

_Comment by @dpgraham4401 on 2025-01-20 14:46_

@charliermarsh, to clarify, you're thinking this should be added this as `D420` since `D419` is the last error code in the [pydocstyle Docstring Content Issues](https://www.pydocstyle.org/en/stable/error_codes.html#grouping)? grouping

---

_Comment by @dhruvmanila on 2025-01-29 08:45_

> to clarify, you're thinking this should be added this as `D420` since `D419` is the last error code in the [pydocstyle Docstring Content Issues](https://www.pydocstyle.org/en/stable/error_codes.html#grouping)?

I'd say yes as `D4XX` are for docstring content issues (https://www.pydocstyle.org/en/stable/error_codes.html#grouping).

---

_Comment by @sebastian-altamirano on 2025-02-13 23:09_

The rule should also work for Google-style docstrings:
- https://google.github.io/styleguide/pyguide.html#383-functions-and-methods
- https://github.com/google/styleguide/issues/847#issuecomment-2513517520

---

_Comment by @dpgraham4401 on 2025-02-16 14:33_

I'm [working on this](https://github.com/dpgraham4401/ruff/blob/numpy_docstring_ordering-9641/crates/ruff_linter/src/rules/pydocstyle/rules/docstring_order.rs), but it'll be my first time contributing to ruff and I'm a novice rustacean so might be a while before a PR 😁

---

_Comment by @dpgraham4401 on 2025-04-04 13:45_

Update, life has gotten in the way and I'm not going to have time to work on this for some time. Sorry y'all. 

---
