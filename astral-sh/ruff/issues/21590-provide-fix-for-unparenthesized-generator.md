---
number: 21590
title: Provide fix for unparenthesized generator expressions
type: issue
state: open
author: injust
labels:
  - fixes
assignees: []
created_at: 2025-11-23T11:19:22Z
updated_at: 2025-11-23T11:30:07Z
url: https://github.com/astral-sh/ruff/issues/21590
synced_at: 2026-01-10T01:23:02Z
---

# Provide fix for unparenthesized generator expressions

---

_Issue opened by @injust on 2025-11-23 11:19_

### Summary

https://play.ruff.rs/802479b3-7442-47f3-9dbf-b5918bc61b3d

```python
max(x for x in foo)
max(x for x in foo, default=0)
# above line should be rewritten to:
max((x for x in foo), default=0)
```

It's common for a generator expression to need parentheses after adding an extra argument. Ruff should provide a fix for this (even if unsafe).

---

_Comment by @MichaReiser on 2025-11-23 11:30_

Makes sense. This will require some infrastructure to add fixes to syntax errors.

---

_Label `fixes` added by @MichaReiser on 2025-11-23 11:30_

---
