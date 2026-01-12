```yaml
number: 1026
title: Missing warnings for break and continue statements outside of loops
type: issue
state: closed
author: correctmost
labels: []
assignees: []
created_at: 2025-08-17T13:37:42Z
updated_at: 2025-08-17T13:40:35Z
url: https://github.com/astral-sh/ty/issues/1026
synced_at: 2026-01-12T15:54:24Z
```

# Missing warnings for break and continue statements outside of loops

---

_@correctmost_

### Summary

ty does not seem to warn about `break` and `continue` statements outside of loops:

```python
break
continue
```

--> https://play.ty.dev/cc42a4fb-75c1-446a-8c0c-c02af2c0df1a

Pyright, Pyrefly, mypy, and ruff issue warnings for both statements.

### Version

ty 0.0.1-alpha.18

---

_Comment by @AlexWaygood on 2025-08-17 13:40_

Thanks for the report! This is covered by https://github.com/astral-sh/ruff/issues/17412 (we'd almost certainly implement the syntax error for Ruff and ty simultaneously -- most of our syntax error detection is shared between the two projects)

---

_Closed by @AlexWaygood on 2025-08-17 13:40_

---
