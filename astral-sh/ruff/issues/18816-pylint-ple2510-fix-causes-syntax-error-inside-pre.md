```yaml
number: 18816
title: "[`Pylint`] `PLE2510` fix causes syntax error inside pre-3.12 f-strings"
type: issue
state: open
author: MeGaGiGaGon
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-06-20T06:44:46Z
updated_at: 2025-06-22T08:11:13Z
url: https://github.com/astral-sh/ruff/issues/18816
synced_at: 2026-01-12T15:54:56Z
```

# [`Pylint`] `PLE2510` fix causes syntax error inside pre-3.12 f-strings

---

_@MeGaGiGaGon_

### Summary

If a backspace is inside an f-string replacement while a pre-3.12 version is targeted, the fix for [invalid-character-backspace (PLE2510)](https://docs.astral.sh/ruff/rules/invalid-character-backspace/#invalid-character-backspace-ple2510) will introduce a syntax error. This happens both inside a string inside the replacement, and inside the format specs. [playground](https://play.ruff.rs/79ed94ae-5031-4d55-a53c-5a7fabe26700)
```
PS D:\python_projects> Get-Content issue.py
```
```py
f"{'␈'}"
```
^ note: I'm cheating here, PowerShell does not actually display the backspace character as `␈`, it actually gets displayed as `f"{'}"`
```
PS D:\python_projects> uvx ruff check issue.py --isolated --select PLE --target-version py39
```
```snap
issue.py:1:5: PLE2510 [*] Invalid unescaped character backspace, use "\b" instead
  |
1 | f"{'␈'}"
  |     ^ PLE2510
  |
  = help: Replace with escape sequence

Found 1 error.
[*] 1 fixable with the `--fix` option.
```
```
PS D:\python_projects> uvx ruff check issue.py --isolated --select PLE --target-version py39 --fix
```
```snap
issue.py:1:5: SyntaxError: Cannot use an escape sequence (backslash) in f-strings on Python 3.9 (syntax was added in Python 3.12)
  |
1 | f"{'\b'}"
  |     ^
  |

Found 2 errors (1 fixed, 1 remaining).
```

This also applies to `PLE2512`, `PLE2513`, and `PLE2515` [playground](https://play.ruff.rs/dc2bc095-7f43-4f4c-ba03-a1167e147196), as well as `PLE2514`, but the playground does not support null chars.

### Version

ruff 0.12.0 (87f0feb 2025-06-17) + playground

---

_Comment by @MichaReiser on 2025-06-20 07:33_

This might be a bit annoying to fix but we can disable the fix if inside an interpolated element and the target version is older than 3.12

---

_Label `bug` added by @MichaReiser on 2025-06-20 07:33_

---

_Label `fixes` added by @MichaReiser on 2025-06-20 07:33_

---

_Label `help wanted` added by @MichaReiser on 2025-06-20 07:33_

---

_Comment by @LaBatata101 on 2025-06-21 21:11_

> This might be a bit annoying to fix but we can disable the fix if inside an interpolated element and the target version is older than 3.12

The `PLE2510` check operates on the token stream, is there anyway of knowing if it's inside an interpolated element just with that information? The only way I could think of was getting the f-string text and using `parse_expression`.

---

_Comment by @MichaReiser on 2025-06-22 08:11_

There are probably two options: 

1. Rewrite to an AST based rule
2. Look for `FSTRING_START` tokens and then ignore the middle tokens, https://play.ruff.rs/19ca2208-50b9-45ad-a41b-240b49eb2f6d

---
