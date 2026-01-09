---
number: 18999
title: Forbid default args for positional arguments
type: issue
state: open
author: tylerlaprade
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-06-27T20:53:15Z
updated_at: 2025-06-28T00:23:29Z
url: https://github.com/astral-sh/ruff/issues/18999
synced_at: 2026-01-07T13:12:16-06:00
---

# Forbid default args for positional arguments

---

_Issue opened by @tylerlaprade on 2025-06-27 20:53_

### Summary

I would like a rule that prevents me from adding default values to arguments that are not keyword-only. Default values lead to tricky situations when there are more than one since you have to supply all arguments in order.

Supposed I have
```python
def foo(bar, baz=None, qux=None):
```

and I want to supply `qux` but not `baz`. My options are:

```python
foo(bar, None, qux)
```

```python
foo(bar, qux=qux)
```

Of these, I greatly prefer the second so that I don't have to supply an unnecessary `None` for `baz`, and because I only use kwargs for keyword-only args. Suppose the default value for `baz` changes to `''` tomorrow. My calling function doesn't want to change the default behavior, yet the first option will still be supplying `None`.

EDIT: I have an idea for an option to make it less strict. If you set a flag to true, it could allow at most one default value for a positional argument.

---

_Label `rule` added by @MichaReiser on 2025-06-27 21:12_

---

_Label `needs-decision` added by @MichaReiser on 2025-06-27 21:12_

---
