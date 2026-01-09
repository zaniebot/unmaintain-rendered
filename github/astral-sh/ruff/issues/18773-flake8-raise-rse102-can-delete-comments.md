---
number: 18773
title: "[flake8-raise] RSE102 can delete comments"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-06-18T23:41:24Z
updated_at: 2025-06-21T17:09:41Z
url: https://github.com/astral-sh/ruff/issues/18773
synced_at: 2026-01-07T13:12:16-06:00
---

# [flake8-raise] RSE102 can delete comments

---

_Issue opened by @MeGaGiGaGon on 2025-06-18 23:41_

### Summary

If there is a comment inside the parentheses, it will be deleted when the fix is applied. Instead the comments should be moved, the fix should be unsafe, or the fix should not apply. [playground](https://play.ruff.rs/3231d1da-f4b0-4bb0-817a-29376482e7c4)
```
PS ~\Desktop\New_folder>Get-Content issue.py
```
```py
raise TypeError(
    # comment
)
```
```
PS ~\Desktop\New_folder>uvx ruff check issue.py --select RSE --fix --diff
```
```diff
--- issue.py
+++ issue.py
@@ -1,3 +1 @@
-raise TypeError(
-    # comment
-)
+raise TypeError

Would fix 1 error.
```

### Version

ruff 0.12.0 (87f0feb21 2025-06-17) + playground

---

_Label `bug` added by @ntBre on 2025-06-19 00:03_

---

_Label `fixes` added by @ntBre on 2025-06-19 00:03_

---

_Label `help wanted` added by @ntBre on 2025-06-19 00:03_

---

_Comment by @chirizxc on 2025-06-19 09:05_

i take this

---

_Comment by @MichaReiser on 2025-06-19 09:16_

Thank you. We should mark the fix as unsafe and document the fix safety.

---

_Referenced in [astral-sh/ruff#18788](../../astral-sh/ruff/pulls/18788.md) on 2025-06-19 09:18_

---

_Closed by @MichaReiser on 2025-06-21 17:09_

---
