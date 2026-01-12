```yaml
number: 21805
title: docstrings on variables and type aliases are not treated as docstrings
type: issue
state: closed
author: DetachHead
labels: []
assignees: []
created_at: 2025-12-05T07:03:24Z
updated_at: 2025-12-05T07:42:17Z
url: https://github.com/astral-sh/ruff/issues/21805
synced_at: 2026-01-12T15:54:58Z
```

# docstrings on variables and type aliases are not treated as docstrings

---

_@DetachHead_

### Summary

```py
# no docstring related errors are reported here:
asdf = 1
"""
a bunch of trailing spaces here that don't get removed by the formatter --->         
"""

# or here:
type Foo = int
"""
a bunch of trailing spaces here that don't get removed by the formatter --->         
"""

# errors are reported here though (D200, D212, D400, D401, D403, D415):
def foo():
    """
    these ones do --->              
    """
```

https://play.ruff.rs/ca57b564-80d5-42bd-a336-4e6ceb258320

### Version

0.14.8

---

_Closed by @MichaReiser on 2025-12-05 07:42_

---
