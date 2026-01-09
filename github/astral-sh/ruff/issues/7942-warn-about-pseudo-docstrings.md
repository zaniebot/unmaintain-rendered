---
number: 7942
title: Warn about pseudo-docstrings
type: issue
state: closed
author: konstin
labels:
  - docstring
assignees: []
created_at: 2023-10-13T13:19:00Z
updated_at: 2024-11-08T07:38:00Z
url: https://github.com/astral-sh/ruff/issues/7942
synced_at: 2026-01-07T13:12:15-06:00
---

# Warn about pseudo-docstrings

---

_Issue opened by @konstin on 2023-10-13 13:19_

The motivation is the following code from cpython:
```python
        if _sys.platform.startswith("aix"):
            """When the name contains ".a(" and ends with ")",
               e.g., "libFOO.a(libFOO.so)" - this is taken to be an
               archive(member) syntax for dlopen(), and the mode is adjusted.
               Otherwise, name is presented to dlopen() as a file argument.
            """
            if name and name.endswith(")") and ".a(" in name:
                mode |= _os.RTLD_MEMBER | _os.RTLD_NOW
```

What looks like a docstring is actually not a docstring, and we not lint or format it has a docstring. Instead we should show a warning that only module, functions and classes can have docstrings ([PEP 257](https://peps.python.org/pep-0257/#what-is-a-docstring)).

---

_Label `docstring` added by @dhruvmanila on 2023-10-29 04:42_

---

_Comment by @InSyncWithFoo on 2024-11-07 23:46_

This seems to be a duplicate of #11292.

---

_Closed by @MichaReiser on 2024-11-08 07:38_

---
