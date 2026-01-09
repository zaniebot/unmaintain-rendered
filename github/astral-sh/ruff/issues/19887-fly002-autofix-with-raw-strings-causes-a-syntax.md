---
number: 19887
title: FLY002 autofix with raw strings causes a syntax error
type: issue
state: closed
author: Spectre5
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-08-13T00:55:56Z
updated_at: 2025-09-18T17:18:30Z
url: https://github.com/astral-sh/ruff/issues/19887
synced_at: 2026-01-07T13:12:16-06:00
---

# FLY002 autofix with raw strings causes a syntax error

---

_Issue opened by @Spectre5 on 2025-08-13 00:55_

### Summary

I get an error with the autofix for FLY002 when one (or more) of the strings in the join are a raw string:

```bash
$ cat ruff_bug.py
'\n'.join([r'line1','line2'])

$ uvx ruff --version
ruff 0.12.8
$ uvx ruff check ruff_bug.py --no-cache --isolated --fix --unsafe-fixes --select FLY002
Installed 1 package in 1ms

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `ruff_bug.py`, the rule codes FLY002, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

ruff_bug.py:1:1: FLY002 Consider f-string instead of string join
  |
1 | '\n'.join([r'line1','line2'])
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ FLY002
  |
  = help: Replace with f-string

Found 1 error.
[*] 1 fixable with the --fix option.
```

### Version

ruff 0.12.8

---

_Renamed from "FLY002 Autofix Bug with Raw Strings (Syntax Error)" to "FLY002 autofix with raw strings causes a syntax error" by @Spectre5 on 2025-08-13 00:57_

---

_Comment by @Spectre5 on 2025-08-13 00:57_

Possibly related to #19837

---

_Comment by @ntBre on 2025-08-13 13:14_

Thanks for the report! Here's a playground [link](https://play.ruff.rs/1bda4909-0799-4fdf-b81f-b5361987b674), and the result of the fix:

```py
r'line1
line2'
```

Not sure if it's related to the other issue, but it's clearly a problem!

---

_Label `bug` added by @ntBre on 2025-08-13 13:14_

---

_Label `fixes` added by @ntBre on 2025-08-13 13:14_

---

_Referenced in [astral-sh/ruff#19883](../../astral-sh/ruff/pulls/19883.md) on 2025-08-20 12:56_

---

_Referenced in [astral-sh/ruff#20197](../../astral-sh/ruff/pulls/20197.md) on 2025-09-01 18:47_

---

_Closed by @ntBre on 2025-09-18 17:18_

---
