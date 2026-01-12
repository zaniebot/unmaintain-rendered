```yaml
number: 21174
title: SIM102 autofix breaks boolean logic by omitting parentheses around OR expressions
type: issue
state: closed
author: hunterjsb
labels: []
assignees: []
created_at: 2025-10-31T19:30:02Z
updated_at: 2025-10-31T19:47:38Z
url: https://github.com/astral-sh/ruff/issues/21174
synced_at: 2026-01-12T15:54:57Z
```

# SIM102 autofix breaks boolean logic by omitting parentheses around OR expressions

---

_@hunterjsb_

### Summary

SIM102 (collapsible-if) autofix breaks boolean logic when the outer condition contains an `or` operator. The fix omits necessary parentheses, changing the logic from `(A or B) and C` to `A or (B and C)` due to operator precedence.

**Before (correct):**
```python
if x or y:
    if z:
        result = True
```

**After Ruff SIM102 autofix (incorrect):**
```python
if x or y and z:  # Missing parentheses around (x or y)
    result = True
```

**Expected:**
```python
if (x or y) and z:
    result = True
```

**Reproduction:**
```python
# Original: Returns False ✓
x, y, z = True, False, False
if x or y:
    if z:
        result = True  # Not executed

# Ruff's fix: Returns True ✗
if x or y and z:  # Evaluates as: x or (y and z) = True or False = True
    result = True  # Executed (BUG!)

# Expected: Returns False ✓
if (x or y) and z:  # Evaluates as: (True or False) and False = False
    result = True  # Not executed
```

This silently changes program logic. Discovered in production code where it caused incorrect business logic execution.

Similar to #12732 (SIM114), which was fixed in PR #12737 by adding proper parenthesization. The same fix is needed for SIM102.

### Version

ruff 0.14.3


---

_Closed by @hunterjsb on 2025-10-31 19:47_

---
