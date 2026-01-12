```yaml
number: 18776
title: "[flake8-simplify] SIM911 fix causes syntax error"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
assignees: []
created_at: 2025-06-19T00:54:31Z
updated_at: 2025-06-23T14:24:48Z
url: https://github.com/astral-sh/ruff/issues/18776
synced_at: 2026-01-12T15:54:56Z
```

# [flake8-simplify] SIM911 fix causes syntax error

---

_@MeGaGiGaGon_

### Summary

`SIM911`'s fix doesn't add padding/parenthesis to the resulting code, causing errors if it's directly next to other code through putting parenthesis around the zip:
 [playground](https://play.ruff.rs/d69299a5-96c5-4726-969e-1ff85a46efe9)
```
PS ~\Desktop\New_folder>Get-Content issue.py
```
```py
flag_stars = {}
for country, stars in(zip)(flag_stars.keys(), flag_stars.values()):...
```
```
PS ~\Desktop\New_folder>uvx ruff check issue.py --isolated --select SIM --fix
```
```

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `issue.py`, the rule codes SIM911, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

issue.py:2:34: SIM911 Use `flag_stars.items()` instead of `(zip)(flag_stars.keys(), flag_stars.values())`
  |
1 | flag_stars = {}
2 | for country, stars in(zip)(flag_stars.keys(), flag_stars.values()):...
  |                      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ SIM911
  |
  = help: Replace `(zip)(flag_stars.keys(), flag_stars.values())` with `flag_stars.items()`

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

### Version

ruff 0.12.0 (87f0feb21 2025-06-17) + playground

---

_Label `bug` added by @ntBre on 2025-06-19 12:51_

---

_Closed by @MichaReiser on 2025-06-23 14:24_

---
