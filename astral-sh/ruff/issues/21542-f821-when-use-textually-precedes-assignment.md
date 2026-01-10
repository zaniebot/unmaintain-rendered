```yaml
number: 21542
title: F821 when use textually precedes assignment
type: issue
state: closed
author: eltoder
labels:
  - question
assignees: []
created_at: 2025-11-20T16:45:09Z
updated_at: 2025-11-20T18:08:23Z
url: https://github.com/astral-sh/ruff/issues/21542
synced_at: 2026-01-10T11:10:00Z
```

# F821 when use textually precedes assignment

---

_Issue opened by @eltoder on 2025-11-20 16:45_

### Summary

```python
def control_flow():
    for i in range(5):
        if i:
            print(x)
        x = i + 1

    return x
```
This reports F821 on print:
```
F821 Undefined name `x`
 --> t.py:4:19
  |
2 |     for i in range(5):
3 |         if i:
4 |             print(x)
  |                   ^
5 |         x = i + 1
```
ruff does not appear to understand control flow and simply follows the order of statements inside of one scope. This usually produces false negatives for unassigned variables (for example, no errors for a variable only assigned in one branch of an `if`). However, it can also produce a false positive inside a loop.

### Version

0.14.5

---

_Comment by @amyreese on 2025-11-20 16:50_

Flake8 produces the same report. I don't think it's feasible for a linter to understand the context that makes this "work", and this sort of code pattern can be dangerous because changing the conditions of the loop potentially changes whether the body of the loop produces a runtime exception.

IMO this is not a "false positive", because the problem (referencing a variable before it has been potentially defined) clearly exists in the code, regardless of whether it triggers a runtime exception.

---

_Label `question` added by @amyreese on 2025-11-20 16:50_

---

_Comment by @MichaReiser on 2025-11-20 17:33_

I agree. You can use a `noqa` comment to suppress this specific instance or assign a value to `x` outside the loop (even if it's `None`) to suppress the error.

---

_Closed by @MichaReiser on 2025-11-20 17:33_

---

_Comment by @eltoder on 2025-11-20 18:08_

@MichaReiser asking people to suppress errors with `noqa` is basically admitting that there's a bug that won't be fixed. It's also a bad habit for people to get into. What's the problem with fixing the bug? It is completely straightforward.

Also, assigning variable to `None` changes its type, so then I'd have to sprinkle `is not None` checks to satisfy mypy.

---
