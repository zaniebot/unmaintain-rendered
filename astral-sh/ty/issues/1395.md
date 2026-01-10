```yaml
number: 1395
title: Unittest.TestCase.fail() does not narrow variable type
type: issue
state: closed
author: Manezki
labels: []
assignees: []
created_at: 2025-10-18T14:20:35Z
updated_at: 2025-10-18T17:06:06Z
url: https://github.com/astral-sh/ty/issues/1395
synced_at: 2026-01-10T02:06:25Z
```

# Unittest.TestCase.fail() does not narrow variable type

---

_Issue opened by @Manezki on 2025-10-18 14:20_

### Summary

Variable type is not narrowed down by checking for `None` value and interrupting execution with `unittest.TestCase.fail()`.
Substituting `unittest.TestCase.fail()` with `return` does exclude None from possible types.

# Expected behavior

After checking for `None` with execution interruption with `unittest.TestCase.fail()` `None` is no longer consider a possible type for the variable.

That is, in the reproduce I would expect to not get 
```Attribute `color` on type `Structure | None` may be missing (possibly-missing-attribute) [Ln 23, Col 9]```

# Reproduce

https://play.ty.dev/d6d62783-54e5-4ada-b1a4-210e56eb40f5

### Version

ty 0.0.1-alpha.23

---

_Comment by @sharkdp on 2025-10-18 17:06_

Thank you for reporting this. There are two things missing right now to support your example. We do not infer types for `self` in methods, yet. That should be resolved very soon. And the other problem here is #690 (check out some issues that are linked to this, with similar problems).

---

_Closed by @sharkdp on 2025-10-18 17:06_

---
