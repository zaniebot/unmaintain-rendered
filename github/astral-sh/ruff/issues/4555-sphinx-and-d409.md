---
number: 4555
title: Sphinx and D409
type: issue
state: closed
author: Nodd
labels:
  - question
  - docstring
assignees: []
created_at: 2023-05-21T01:55:17Z
updated_at: 2023-05-22T13:37:52Z
url: https://github.com/astral-sh/ruff/issues/4555
synced_at: 2026-01-07T13:12:14-06:00
---

# Sphinx and D409

---

_Issue opened by @Nodd on 2023-05-21 01:55_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
I have this nice code, which passes fine with ruff:
```python
def foo(a):
    """Summary

    Args:
    ----
    a: list
        parameter a
    """
```

The problem is that Sphinx will throw a warning when building the documentation:

```
***/lib/python3.11/site-packages/numpydoc/docscrape.py:460: UserWarning: potentially wrong underline length... 
Args: 
---- in 
***
... in the docstring of foo in ***.
  warn(msg)
```

Sphinx condiders that the comma is part of the title, so wants this:
```python
def foo(a):
    """Summary

    Args:
    -----
    a: list
        parameter a
    """
```
but ruff is now unhappy with D409. I think that in this case ruff is wrong and Sphinx is right, the whole title should be underlined, including the colon.

Note that if I remove the colon, ruff triggers D416, so it's not a workaround.

This is with ruff 0.0.265.

---

_Comment by @charliermarsh on 2023-05-21 15:51_

Interesting... We have the same behavior as pydocstyle -- do you know if this has ever been reported or discussed in pydocstyle?

---

_Label `question` added by @charliermarsh on 2023-05-21 15:51_

---

_Label `docstring` added by @charliermarsh on 2023-05-21 15:51_

---

_Comment by @Nodd on 2023-05-22 08:57_

I couldn't find it. I'll double-check the different docstring styles, it seems that when using `Args:` the title shouldn't be underlined at all.

---

_Comment by @charliermarsh on 2023-05-22 13:23_

What I'm seeing is that the NumPy standard actually wants you to omit the `:`, not increase the length of the underline. That's what I see in all the official examples.

---

_Comment by @Nodd on 2023-05-22 13:37_

D409 and D416 correspond to different docstring styles : https://github.com/PyCQA/pydocstyle/blob/master/docs/error_codes.rst. From this link: D416 is incompatible with `numpy` style, while D409 is incompatible with `google` style. I'll have to choose one or the other.

---

_Closed by @Nodd on 2023-05-22 13:37_

---
