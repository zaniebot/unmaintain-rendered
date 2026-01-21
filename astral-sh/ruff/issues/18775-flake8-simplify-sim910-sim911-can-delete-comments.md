```yaml
number: 18775
title: "[flake8-simplify] SIM910/SIM911 can delete comments"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - fixes
  - help wanted
assignees: []
created_at: 2025-06-19T00:40:13Z
updated_at: 2026-01-21T18:16:03Z
url: https://github.com/astral-sh/ruff/issues/18775
synced_at: 2026-01-21T19:04:52Z
```

# [flake8-simplify] SIM910/SIM911 can delete comments

---

_@MeGaGiGaGon_

### Summary

The fix for `SIM910` will delete comments inside the `get`. If there are comments, they should be moved, the fix should be marked unsafe, or the fix should not apply. [playground](https://play.ruff.rs/f86e679b-f3a2-4b66-95cc-fee74fc212d9)
```
PS ~\Desktop\New_folder>Get-Content issue.py
```
```py
ages = {"Tom": 23, "Maria": 23, "Dog": 11}
age = ages.get( #1
"Cat" #2
, #3
None #4
# 5
)
```
```
PS ~\Desktop\New_folder>uvx ruff check issue.py --isolated --select SIM --fix --diff
```
```diff
--- issue.py
+++ issue.py
@@ -1,7 +1,2 @@
 ages = {"Tom": 23, "Maria": 23, "Dog": 11}
-age = ages.get( #1
-"Cat" #2
-, #3
-None #4
-# 5
-)
+age = ages.get("Cat")

Would fix 1 error.
```

This also applies to `SIM911` [playground](https://play.ruff.rs/9f41cec7-fd4a-4dc2-96b0-00bfab4d3f90)

### Version

ruff 0.12.0 (87f0feb21 2025-06-17) + playground

---

_Renamed from "[flake8-simplify] SIM910 can delete comments" to "[flake8-simplify] SIM910/SIM911 can delete comments" by @MeGaGiGaGon on 2025-06-19 00:45_

---

_Comment by @chirizxc on 2025-06-19 09:27_

i take this

---

_Label `fixes` added by @MichaReiser on 2025-06-19 09:38_

---

_Label `help wanted` added by @MichaReiser on 2025-06-19 09:38_

---

_Comment by @eureka928 on 2026-01-15 12:28_

@MichaReiser I assume this is still opening? may I take up this?

---

_Comment by @MichaReiser on 2026-01-15 12:50_

Yeah sure. The easiest fix would be to mark the fix as unsafe if it intersects with any comment as we do in many other rules. A more advanced fix would be trying to preserve the comments.

---

_Comment by @eureka928 on 2026-01-15 14:26_

PR is ready based on your comment

---

_Closed by @ntBre on 2026-01-21 18:16_

---
