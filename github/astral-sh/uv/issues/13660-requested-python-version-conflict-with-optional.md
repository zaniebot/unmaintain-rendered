---
number: 13660
title: requested python version conflict with optional dependencies
type: issue
state: closed
author: bathal1
labels: []
assignees: []
created_at: 2025-05-26T12:42:46Z
updated_at: 2025-05-26T16:39:14Z
url: https://github.com/astral-sh/uv/issues/13660
synced_at: 2026-01-07T13:12:18-06:00
---

# requested python version conflict with optional dependencies

---

_Issue opened by @bathal1 on 2025-05-26 12:42_

### Summary

It seems that running any python command in a project will try to enforce python version requirements from all dependencies, including optional ones. For example, if we modify the startup project's `pyproject.toml`  generated from `uv init` with the following:

```
[project]
name = "myproject"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.8"
dependencies = []

[project.optional-dependencies]
    doc = [
        "sphinx==8.1.3",
        ]
```

running `uv run main.py` will result in the following error:
```
Using CPython 3.13.3
Creating virtual environment at: .venv
  × No solution found when resolving dependencies for split (python_full_version >= '3.8' and python_full_version < '3.10'):
  ╰─▶ Because the requested Python version (>=3.8) does not satisfy Python>=3.10 and sphinx==8.1.3 depends on Python>=3.10, we can conclude that
      sphinx==8.1.3 cannot be used.
      And because myproject[doc] depends on sphinx==8.1.3 and your project requires myproject[doc], we can conclude that your project's
      requirements are unsatisfiable.

      hint: The `requires-python` value (>=3.8) includes Python versions that are not supported by your dependencies (e.g., sphinx==8.1.3 only
      supports >=3.10). Consider using a more restrictive `requires-python` value (like >=3.10).
```

This is somewhat restrictive as the main part of a package could be compatible with a lower version of python, while some extra part (e.g. documentation) may need a newer one. Is there any way to specify that the optional dependencies should not matter in this case ?

### Platform

Ubuntu 24.04 x86_64

### Version

uv 0.7.6

### Python version

3.13.3

---

_Label `bug` added by @bathal1 on 2025-05-26 12:42_

---

_Comment by @konstin on 2025-05-26 12:53_

We recommend using dependency groups for tooling dependencies such as documentation, and we plan to allow defining a separate requires-python for dependency groups (https://github.com/astral-sh/uv/issues/11606)

---

_Label `bug` removed by @konstin on 2025-05-26 12:53_

---

_Comment by @charliermarsh on 2025-05-26 16:39_

I believe you're looking for:

```toml
[project]
name = "myproject"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.8"
dependencies = []

[project.optional-dependencies]
doc = [
  "sphinx==8.1.3 ; python_version >= '3.10'",
]
```

As @konstin said, we plan to allow users to set this at the group level in the future.

---

_Closed by @charliermarsh on 2025-05-26 16:39_

---
