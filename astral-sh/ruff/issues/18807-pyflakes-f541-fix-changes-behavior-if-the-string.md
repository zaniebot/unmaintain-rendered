```yaml
number: 18807
title: "[`Pyflakes`] `F541` fix changes behavior if the string becomes a docstring"
type: issue
state: open
author: MeGaGiGaGon
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-06-19T23:30:02Z
updated_at: 2025-06-20T06:59:31Z
url: https://github.com/astral-sh/ruff/issues/18807
synced_at: 2026-01-10T11:09:58Z
```

# [`Pyflakes`] `F541` fix changes behavior if the string becomes a docstring

---

_Issue opened by @MeGaGiGaGon on 2025-06-19 23:30_

### Summary

If the string [f-string-missing-placeholders (F541)](https://docs.astral.sh/ruff/rules/f-string-missing-placeholders/#f-string-missing-placeholders-f541) is targeting is in a position where it would become a docstring, it can change behavior. It should probably be marked unsafe or not apply in this case.
[playground](https://play.ruff.rs/1a928179-19c9-4a39-9649-a911adb054cf)
```
PS ~\Desktop\New_folder\ruff>Get-Content issue.py
```
```py
f"Hello, world!"
print(__doc__)
```
```
PS ~\Desktop\New_folder\ruff>py issue.py
```
```
None
```
```
PS ~\Desktop\New_folder\ruff>uvx ruff check issue.py --isolated --select F
```
```snap
issue.py:1:1: F541 [*] f-string without any placeholders
  |
1 | f"Hello, world!"
  | ^^^^^^^^^^^^^^^^ F541
2 | print(__doc__)
  |
  = help: Remove extraneous `f` prefix

Found 1 error.
[*] 1 fixable with the `--fix` option.
```
```
PS ~\Desktop\New_folder\ruff>uvx ruff check issue.py --isolated --select F --fix
```
```
Found 1 error (1 fixed, 0 remaining).
```
```
PS ~\Desktop\New_folder\ruff>Get-Content issue.py
```
```py
"Hello, world!"
print(__doc__)
```
```
PS ~\Desktop\New_folder\ruff>py issue.py
```
```
Hello, world!
```

### Version

ruff 0.12.0 (87f0feb 2025-06-17) + playground

---

_Label `bug` added by @MichaReiser on 2025-06-20 06:59_

---

_Label `fixes` added by @MichaReiser on 2025-06-20 06:59_

---

_Comment by @MichaReiser on 2025-06-20 06:59_

Thanks. I think we shouldn't provide a fix in this (rare) case

---
