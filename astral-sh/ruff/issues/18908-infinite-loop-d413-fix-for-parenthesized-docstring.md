```yaml
number: 18908
title: "[Infinite loop] D413 fix for parenthesized docstring"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - docstring
  - fixes
assignees: []
created_at: 2025-06-24T05:06:59Z
updated_at: 2025-06-30T14:49:15Z
url: https://github.com/astral-sh/ruff/issues/18908
synced_at: 2026-01-10T11:09:59Z
```

# [Infinite loop] D413 fix for parenthesized docstring

---

_Issue opened by @dscorbett on 2025-06-24 05:06_

### Summary

The fix for [`missing-blank-line-after-last-section` (D413)](https://docs.astral.sh/ruff/rules/missing-blank-line-after-last-section/) fails to converge when the docstring is parenthesized and the opening parenthesis appears on the same line as the initial quotation mark. `compute_indentation` detects the parenthesis as part of the indentation. Some rules use `clean_space` to deal with this.

[Playground](https://play.ruff.rs/112e04e1-61c9-4897-8ef6-6bd61e8d4ad5)

```console
$ cat >d413.py <<'# EOF'
def get_random_number():
    ("""Return a random number.

    Returns:
        A random number.
    """)
    return 4
# EOF

$ ruff --isolated check d413.py --select D413 --diff

error: Failed to converge after 100 iterations.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `d413.py`, the rule codes D413, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

--- d413.py
+++ d413.py
@@ -3,5 +3,204 @@
 
     Returns:
         A random number.
-    """)
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (
+
+    (""")
     return 4

Would fix 100 errors.
```

### Version

ruff 0.12.0 (87f0feb21 2025-06-17)

---

_Label `bug` added by @AlexWaygood on 2025-06-24 10:05_

---

_Label `docstring` added by @AlexWaygood on 2025-06-24 10:05_

---

_Label `fixes` added by @AlexWaygood on 2025-06-24 10:05_

---

_Closed by @ntBre on 2025-06-30 14:49_

---
