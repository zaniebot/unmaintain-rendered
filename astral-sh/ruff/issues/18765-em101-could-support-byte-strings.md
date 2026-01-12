```yaml
number: 18765
title: EM101 could support byte strings
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - rule
  - help wanted
assignees: []
created_at: 2025-06-18T18:23:07Z
updated_at: 2025-06-25T14:53:57Z
url: https://github.com/astral-sh/ruff/issues/18765
synced_at: 2026-01-12T15:54:56Z
```

# EM101 could support byte strings

---

_@MeGaGiGaGon_

### Summary

EM101 currently doesn't flag errors that have byte strings, despite them still causing a duplicate message [playground](https://play.ruff.rs/a4183043-bc45-49c1-88ff-dc5b75310d29)
```
PS ~\Desktop\New_folder>Get-Content issue.py
```
```py
raise RuntimeError(b"error message")  # no error
raise RuntimeError("error message")  # EM101
```
```
PS ~\Desktop\New_folder>uvx ruff check issue.py --select EM
```
```
issue.py:2:20: EM101 Exception must not use a string literal, assign to variable first
  |
1 | raise RuntimeError(b"error message")  # no error
2 | raise RuntimeError("error message")  # EM101
  |                    ^^^^^^^^^^^^^^^ EM101
  |
  = help: Assign to variable; remove string literal

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```
```
PS ~\Desktop\New_folder>py issue.py
```
```
Traceback (most recent call last):
  File "~\Desktop\New_folder\issue.py", line 1, in <module>
    raise RuntimeError(b"error message")  # no error
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
RuntimeError: b'error message'
```
A [github search](https://github.com/search?q=language%3APython+raise+RuntimeError%28b%22&type=code) for `raise RuntimeError(b"` gives a decent number of results, so this is not unheard of.

---

_Label `rule` added by @MichaReiser on 2025-06-19 09:32_

---

_Label `help wanted` added by @MichaReiser on 2025-06-19 09:33_

---

_Closed by @ntBre on 2025-06-25 14:53_

---
