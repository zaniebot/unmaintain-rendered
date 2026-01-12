```yaml
number: 17967
title: "Autofix `S608` when fstring has no placeholders"
type: issue
state: closed
author: dosisod
labels:
  - rule
  - help wanted
assignees: []
created_at: 2025-05-08T21:22:09Z
updated_at: 2025-05-10T10:37:59Z
url: https://github.com/astral-sh/ruff/issues/17967
synced_at: 2026-01-12T15:54:56Z
```

# Autofix `S608` when fstring has no placeholders

---

_@dosisod_

### Summary

I ran into this on one of my codebases today:

```python
from sqlite3 import connect

db = connect(":memory:")

db.execute(f"SELECT * FROM table").fetchall()
```

Running with ruff v0.11.8:

```
$ ruff check x.py --select S608,F541
x.py:5:12: F541 [*] f-string without any placeholders
  |
3 | db = connect(":memory:")
4 |
5 | db.execute(f"SELECT * FROM table").fetchall()
  |            ^^^^^^^^^^^^^^^^^^^^^^ F541
  |
  = help: Remove extraneous `f` prefix

x.py:5:12: S608 Possible SQL injection vector through string-based query construction
  |
3 | db = connect(":memory:")
4 |
5 | db.execute(f"SELECT * FROM table").fetchall()
  |            ^^^^^^^^^^^^^^^^^^^^^^ S608
  |

Found 2 errors.
[*] 1 fixable with the `--fix` option.
```

In this case, fixing F541 by removing the `f` will also fix S608, which means you can do one of the following:
* Emit both errors, and mark them both as fixable
* Emit just S608, and mark it as fixable (by applying the fix logic from S541)

It might be more effort than it's worth to improve this, but it would be an easy case to detect in S608.

### Version

ruff 0.11.8

---

_Label `rule` added by @MichaReiser on 2025-05-09 06:11_

---

_Label `help wanted` added by @MichaReiser on 2025-05-09 06:11_

---

_Label `h]` added by @MichaReiser on 2025-05-09 06:11_

---

_Label `h]` removed by @MichaReiser on 2025-05-09 06:11_

---

_Comment by @MichaReiser on 2025-05-09 06:11_

Skipping `S608` for f-strings without any placeholders seems a good idea

---

_Closed by @MichaReiser on 2025-05-10 10:37_

---
