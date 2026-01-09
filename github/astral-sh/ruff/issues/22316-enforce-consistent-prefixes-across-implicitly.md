---
number: 22316
title: Enforce consistent prefixes across implicitly concatenated strings
type: issue
state: open
author: GideonBear
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-12-31T13:09:27Z
updated_at: 2025-12-31T15:13:23Z
url: https://github.com/astral-sh/ruff/issues/22316
synced_at: 2026-01-07T13:12:16-06:00
---

# Enforce consistent prefixes across implicitly concatenated strings

---

_Issue opened by @GideonBear on 2025-12-31 13:09_

### Summary

Ref #4662 and #10885
@charliermarsh, pinging you since this was originally a comment in #4662, but I decided to flesh this follow-up question out into a feature request immediately.

This has to do with rule [f-string-missing-placeholders (F541)](https://docs.astral.sh/ruff/rules/f-string-missing-placeholders/#f-string-missing-placeholders-f541), but should work for other prefixes (`r"..."`) as well.

---

If the stance of ruff is "you should/can use common prefixes", there should (IMO) be a rule that enforces this, to strive for consistency.
```py
# Bad, inconsistent
test_string = (
    f"this is a string without placeholders "
    "and this is also without placeholders "
    f"{some_string} is in a placeholder"
)
# Fine
test_string = (
    "this is a string without placeholders "
    f"{some_string} is in a placeholder"
)
```
Proposal:
```diff
 # Bad, inconsistent
 test_string = (
     f"this is a string without placeholders "
-    "and this is also without placeholders "
+    f"and this is also without placeholders "
     f"{some_string} is in a placeholder"
 )
 # Fine
 test_string = (
-    "this is a string without placeholders "
+    f"this is a string without placeholders "
     f"{some_string} is in a placeholder"
 )
```

---

_Label `rule` added by @ntBre on 2025-12-31 15:13_

---

_Label `needs-decision` added by @ntBre on 2025-12-31 15:13_

---

_Comment by @ntBre on 2025-12-31 15:13_

This makes sense to me! I'll put `needs-decision` on it for now to let others have a chance to weigh in, but this seems like a reasonable stylistic rule.

cc @amyreese 

---
