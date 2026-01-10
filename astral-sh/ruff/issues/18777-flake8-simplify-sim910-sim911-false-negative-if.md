```yaml
number: 18777
title: "[flake8-simplify] SIM910/SIM911 false negative if variable is named dict"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
assignees: []
created_at: 2025-06-19T01:04:57Z
updated_at: 2025-06-20T17:25:37Z
url: https://github.com/astral-sh/ruff/issues/18777
synced_at: 2026-01-10T11:09:58Z
```

# [flake8-simplify] SIM910/SIM911 false negative if variable is named dict

---

_Issue opened by @MeGaGiGaGon on 2025-06-19 01:04_

### Summary

`SIM911` has a false negative if the dict is named `dict` [playground](https://play.ruff.rs/856a5d41-547e-4537-8958-891966c3d97b)
```
PS ~\Desktop\New_folder>Get-Content issue.py
```
```py
dict = {}
for country, stars in zip(dict.keys(), dict.values()):...
```
```
PS ~\Desktop\New_folder>uvx ruff check issue.py --isolated --select SIM
```
```
All checks passed!
```

This also applies to `SIM910` [playground](https://play.ruff.rs/b76b7679-7f6f-4430-b694-f6629efa4efe)

### Version

ruff 0.12.0 (87f0feb 2025-06-17) + playground

---

_Renamed from "[flake8-simplify] SIM911 false negative if variable is named dict" to "[flake8-simplify] SIM910/SIM911 false negative if variable is named dict" by @MeGaGiGaGon on 2025-06-19 01:17_

---

_Label `bug` added by @ntBre on 2025-06-19 12:51_

---

_Comment by @LaBatata101 on 2025-06-19 14:25_

I'm working on this one

---

_Assigned to @LaBatata101 by @ntBre on 2025-06-20 13:14_

---

_Closed by @ntBre on 2025-06-20 17:25_

---
