---
number: 18783
title: "[Perflint] PERF101 fix can cause syntax error"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-06-19T06:55:01Z
updated_at: 2025-06-23T11:51:47Z
url: https://github.com/astral-sh/ruff/issues/18783
synced_at: 2026-01-10T01:23:00Z
---

# [Perflint] PERF101 fix can cause syntax error

---

_Issue opened by @MeGaGiGaGon on 2025-06-19 06:55_

### Summary

If the `list` is next to another item using parenthesis, fixing `PERF101` can cause a syntax error. The fix should make sure there is always padding. [playground](https://play.ruff.rs/3ae00ded-2cad-4739-a0c4-eab55e23746e)
```
PS D:\python_projects> Get-Content issue.py
```
```py
items = (1, 2, 3)
for i in(list)(items):
    print(i)
```
```
PS D:\python_projects> uvx ruff check issue.py --select PERF --fix
```
```

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `issue.py`, the rule codes PERF101, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

issue.py:2:9: PERF101 Do not cast an iterable to `list` before iterating over it
  |
1 | items = (1, 2, 3)
2 | for i in(list)(items):
  |         ^^^^^^^^^^^^^ PERF101
3 |     print(i)
  |
  = help: Remove `list()` cast

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

### Version

ruff 0.12.0 (87f0feb21 2025-06-17) + playground

---

_Label `bug` added by @MichaReiser on 2025-06-19 09:17_

---

_Label `help wanted` added by @MichaReiser on 2025-06-19 09:17_

---

_Comment by @LaBatata101 on 2025-06-19 14:44_

I'm working on this

---

_Referenced in [astral-sh/ruff#18803](../../astral-sh/ruff/pulls/18803.md) on 2025-06-19 20:00_

---

_Assigned to @LaBatata101 by @ntBre on 2025-06-20 13:13_

---

_Closed by @MichaReiser on 2025-06-23 11:51_

---
