---
number: 14791
title: "Various rules refer to `except*` as `except`"
type: issue
state: closed
author: jakkdl
labels:
  - good first issue
  - help wanted
  - diagnostics
assignees: []
created_at: 2024-12-05T14:39:16Z
updated_at: 2024-12-08T17:37:36Z
url: https://github.com/astral-sh/ruff/issues/14791
synced_at: 2026-01-07T13:12:16-06:00
---

# Various rules refer to `except*` as `except`

---

_Issue opened by @jakkdl on 2024-12-05 14:39_

I did a pass on all flake8-bugbear rules upstream to check for `except*` handling (https://github.com/PyCQA/flake8-bugbear/pull/500), and then also checked ruff.. which already handles all of them 🥳 But a bunch of them assume `except` in their message, which could perhaps be confusing to users - but tbh it's mostly a nitpick.

I have not checked any rules other than flake8-bugbear, so I suggest you do a pass checking for any other rules that might be referring to `except`.

```python
try:
    a = 1
except* ValueError:
    a = 2
except* ValueError:
    a = 2

try:
    pass
except* ():
    pass

try:
    pass
except* 1:  # error
    pass

try:
    raise ValueError
except* ValueError:
    raise UserWarning
```

```
$ ruff check --select=B foo.py
foo.py:6:9: B025 try-except block with duplicate exception `ValueError`
foo.py:11:1: B029 Using `except ():` with an empty tuple does not catch anything; add exceptions to handle
foo.py:16:9: B030 `except` handlers should only be exception classes or tuples of exception classes
foo.py:22:5: B904 Within an `except` clause, raise exceptions with `raise ... from err` or `raise ... from None` to distinguish them from errors in exception handling
Found 4 errors.
```

expected output

```
$ ruff check --select=B foo.py
foo.py:6:9: B025 try-except* block with duplicate exception `ValueError`
foo.py:11:1: B029 Using `except* ():` with an empty tuple does not catch anything; add exceptions to handle
foo.py:16:9: B030 `except*` handlers should only be exception classes or tuples of exception classes
foo.py:22:5: B904 Within an `except*` clause, raise exceptions with `raise ... from err` or `raise ... from None` to distinguish them from errors in exception handling
Found 4 errors.
```

---

_Label `diagnostics` added by @dylwil3 on 2024-12-05 16:14_

---

_Comment by @dylwil3 on 2024-12-05 16:15_

Thanks for the audit and write-up! This seems reasonable to me to try to fix.

---

_Label `help wanted` added by @dylwil3 on 2024-12-05 16:15_

---

_Label `good first issue` added by @dylwil3 on 2024-12-05 16:15_

---

_Comment by @smokyabdulrahman on 2024-12-06 08:58_

I can take this if it's available 👌

---

_Assigned to @smokyabdulrahman by @MichaReiser on 2024-12-06 09:13_

---

_Referenced in [astral-sh/ruff#14815](../../astral-sh/ruff/pulls/14815.md) on 2024-12-06 12:40_

---

_Comment by @smokyabdulrahman on 2024-12-06 12:43_

Draft PR opened.
You can comment anytime if my approach doesn't meet the standard of `astral-sh` or `rust`.

P.S. It's my first rust code base contribution. 

---

_Comment by @smokyabdulrahman on 2024-12-06 20:42_

[PR Ready for Review](https://github.com/astral-sh/ruff/pull/14815)

---

_Closed by @MichaReiser on 2024-12-08 17:37_

---

_Closed by @MichaReiser on 2024-12-08 17:37_

---
