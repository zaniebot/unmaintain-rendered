```yaml
number: 16292
title: Is it possible to declare conflicting dependencies against published dependencies?
type: issue
state: closed
author: winstxnhdw
labels:
  - question
assignees: []
created_at: 2025-10-14T05:28:05Z
updated_at: 2025-12-28T18:53:23Z
url: https://github.com/astral-sh/uv/issues/16292
synced_at: 2026-01-12T16:02:28Z
```

# Is it possible to declare conflicting dependencies against published dependencies?

---

_@winstxnhdw_

### Question

I want my users to be able to install my package with the default portable `scipy` but also give them the option to install the Intel MKL accelerated `scipy`. Trying to to do this without defining `conflicts` leads to a conflicts error. This doesn't seem to be documented either.

```toml
[project]
requires-python = ">=3.9"
dependencies = ["scipy>=1.11.0", "scipy>=1.13.0; python_version >= '3.12'"]

[project.optional-dependencies]
intel = ["scipy>=1.13.1; python_version >= '3.11' and python_version < '3.13'"]

[tool.uv]
conflicts = [[{ extra = "intel" }, { group = "default" }]]  # declare default is conflicting
index = [
    { name = "intel", url = "https://software.repos.intel.com/python/pypi", explicit = true },
    { name = "pypi", url = "https://pypi.org/simple" },
]

[tool.uv.sources]
scipy = [
    { index = "intel", extra = "intel" },
    { index = "pypi" },
]
```

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @winstxnhdw on 2025-10-14 05:28_

---

_Comment by @konstin on 2025-12-18 16:26_

We only support conflicts between two optional items, e.g. two extras, not between the mandatory default and an extra.

---

_Closed by @winstxnhdw on 2025-12-28 18:53_

---
