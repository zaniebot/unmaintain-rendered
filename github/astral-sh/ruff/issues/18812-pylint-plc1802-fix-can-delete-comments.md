---
number: 18812
title: "[`Pylint`] `PLC1802` fix can delete comments"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-06-20T05:39:10Z
updated_at: 2025-06-23T00:32:59Z
url: https://github.com/astral-sh/ruff/issues/18812
synced_at: 2026-01-07T13:12:16-06:00
---

# [`Pylint`] `PLC1802` fix can delete comments

---

_Issue opened by @MeGaGiGaGon on 2025-06-20 05:39_

### Summary

The fix for [len-test (PLC1802)](https://docs.astral.sh/ruff/rules/len-test/#len-test-plc1802) can delete comments. It should instead either be an unsafe fix in that case, not apply, or move the comments. [playground](https://play.ruff.rs/98d97f92-bcbe-4c18-846f-6874610a82a7)
```
PS D:\python_projects> Get-Content issue.py
```
```py
fruits = []
if (  # 1
    len  # 2
    )(  # 3
        fruits  # 4
    ):
    ...
```
```
PS D:\python_projects> uvx ruff check issue.py --select PLC
```
```snap
issue.py:2:4: PLC1802 [*] `len(fruits)` used as condition without comparison
  |
1 |   fruits = []
2 |   if (  # 1
  |  ____^
3 | |     len  # 2
4 | |     )(  # 3
5 | |         fruits  # 4
6 | |     ):
  | |_____^ PLC1802
7 |       ...
  |
  = help: Remove `len`

Found 1 error.
[*] 1 fixable with the `--fix` option.
```
```
PS D:\python_projects> uvx ruff check issue.py --select PLC --fix --diff
```
```diff
--- issue.py
+++ issue.py
@@ -1,7 +1,3 @@
 fruits = []
-if (  # 1
-    len  # 2
-    )(  # 3
-        fruits  # 4
-    ):
+if fruits:
     ...

Would fix 1 error.
```

### Version

ruff 0.12.0 (87f0feb 2025-06-17) + playground

---

_Label `bug` added by @MichaReiser on 2025-06-20 06:13_

---

_Label `fixes` added by @MichaReiser on 2025-06-20 06:13_

---

_Label `help wanted` added by @MichaReiser on 2025-06-20 06:13_

---

_Comment by @MichaReiser on 2025-06-20 06:13_

The fix should be marked as unsafe if it intersects with any comments and the fix safety should be documented.

---

_Comment by @LaBatata101 on 2025-06-20 20:40_

I'm working on this

---

_Assigned to @LaBatata101 by @ntBre on 2025-06-20 20:46_

---

_Referenced in [astral-sh/ruff#18836](../../astral-sh/ruff/pulls/18836.md) on 2025-06-20 21:17_

---

_Closed by @charliermarsh on 2025-06-23 00:32_

---
