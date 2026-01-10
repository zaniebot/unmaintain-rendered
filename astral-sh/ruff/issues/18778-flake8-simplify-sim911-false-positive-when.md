```yaml
number: 18778
title: "[flake8-simplify] SIM911 false positive when methods have args"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
assignees: []
created_at: 2025-06-19T01:16:30Z
updated_at: 2025-10-21T20:58:50Z
url: https://github.com/astral-sh/ruff/issues/18778
synced_at: 2026-01-10T11:09:58Z
```

# [flake8-simplify] SIM911 false positive when methods have args

---

_Issue opened by @MeGaGiGaGon on 2025-06-19 01:16_

### Summary

`SIM911` has a false positive if you give the methods args, since they are removed when it is transformed into `.items()`, which changes the behavior of the code to no longer raise a `TypeError`. If there is a situation in where the code thinks the variable is a `dict`, but it's actually the `dict` class, that could actually show up in non-constructed code. I was unable to find such a situation though. [playground](https://play.ruff.rs/fb6f3473-53a1-401b-88ea-a943232e4b44)
```
PS ~\Desktop\New_folder>Get-Content issue.py
```
```py
d = {}
for country, stars in zip(d.keys(*x), d.values("hello")):...
```
```
PS ~\Desktop\New_folder>uvx ruff check issue.py --isolated --select SIM --fix --diff
```
```diff
--- issue.py
+++ issue.py
@@ -1,2 +1,2 @@
 d = {}
-for country, stars in zip(d.keys(*x), d.values("hello")):...
+for country, stars in d.items():...

Would fix 1 error.
```

### Version

ruff 0.12.0 (87f0feb 2025-06-17) + playground

---

_Label `bug` added by @ntBre on 2025-06-19 12:50_

---

_Closed by @ntBre on 2025-10-21 20:58_

---
