---
number: 17208
title: Report unused private functions and variables
type: issue
state: closed
author: sk-
labels: []
assignees: []
created_at: 2025-04-04T16:06:21Z
updated_at: 2025-04-04T18:55:18Z
url: https://github.com/astral-sh/ruff/issues/17208
synced_at: 2026-01-07T13:12:16-06:00
---

# Report unused private functions and variables

---

_Issue opened by @sk- on 2025-04-04 16:06_

### Summary

Pylance has a nice feature in which unused private functions and variables are reported as unused.

For example the following code triggers two issues reported by Pylance:
- "_UNUSED_CONSTANT" is not accessed
- "_unused_function" is not accessed

```python
_UNUSED_CONSTANT = 3


def _unused_function() -> None:
    pass
```

However, ruff does not flag any of the code (except for missing docs) even when running all the checks

```bash
uv run ruff check --select ALL test_unused.py
test_unused.py:1:1: D100 Missing docstring in public module
test_unused.py:1:1: CPY001 Missing copyright notice at top of file
Found 2 errors.
```

---

_Comment by @ntBre on 2025-04-04 17:41_

This does sound like a nice feature, but I think it's a duplicate of #872 and needs type inference.

---

_Closed by @ntBre on 2025-04-04 17:41_

---

_Comment by @sk- on 2025-04-04 18:55_

@ntBre note that this is a smaller feature than what is requested in #872, as it focuses only on private symbols which should only be used within the defining file. 

So in order to implement this feature there is no need to implement multi file analysis (https://github.com/astral-sh/ruff/issues/872#issuecomment-1955919200)

---
