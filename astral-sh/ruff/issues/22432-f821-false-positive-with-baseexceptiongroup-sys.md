```yaml
number: 22432
title: "F821 false positive with `BaseExceptionGroup` + `sys.version_info >= (3, 11)`"
type: issue
state: closed
author: Liam-DeVoe
labels: []
assignees: []
created_at: 2026-01-07T06:26:22Z
updated_at: 2026-01-07T07:28:36Z
url: https://github.com/astral-sh/ruff/issues/22432
synced_at: 2026-01-10T11:10:00Z
```

# F821 false positive with `BaseExceptionGroup` + `sys.version_info >= (3, 11)`

---

_Issue opened by @Liam-DeVoe on 2026-01-07 06:26_

### Summary

```python
import sys

if sys.version_info >= (3, 11):
    _ = BaseExceptionGroup
```

`ruff check` reports:

```
F821 Undefined name `BaseExceptionGroup`. Consider specifying `requires-python = ">= 3.11"` or `tool.ruff.target-version = "py311"` in your `pyproject.toml` file.
 --> sandbox.py:4:9
  |
3 | if sys.version_info >= (3, 11):
4 |     _ = BaseExceptionGroup
  |         ^^^^^^^^^^^^^^^^^^
  |

Found 1 error.
```

`BaseExceptionGroup` was added in 3.11, and shouldn't be reported as an error.

I don't know if ruff guarantees this kind of static analysis for `sys.version_info` checks. IIRC I've seen ruff be smart about other version-guarded analysis, which is why I'm filing this as a bug report. If not, consider this a feature request :-) 

### Version

ruff 0.14.10

---

_Comment by @MichaReiser on 2026-01-07 07:28_

Thanks, this is  https://github.com/astral-sh/ruff/issues/13296

---

_Closed by @MichaReiser on 2026-01-07 07:28_

---
