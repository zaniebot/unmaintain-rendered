---
number: 16488
title: "New rule request: unused function"
type: issue
state: closed
author: Garrett-R
labels: []
assignees: []
created_at: 2025-03-04T00:43:53Z
updated_at: 2025-03-04T03:37:59Z
url: https://github.com/astral-sh/ruff/issues/16488
synced_at: 2026-01-07T13:12:16-06:00
---

# New rule request: unused function

---

_Issue opened by @Garrett-R on 2025-03-04 00:43_

### Summary

Similar to [F841: unused-variable](https://docs.astral.sh/ruff/rules/#pyflakes-f), it would be nice to have a `unused-function` rule.

For example, it could catch:

```py
# foo.py

def bar():
    pass
```

if that's not important anywhere in repo.  This rule of course wouldn't be used by libraries whose functions are used outside the repo.

---

_Comment by @KRRT7 on 2025-03-04 00:49_

sounds like something that https://pypi.org/project/vulture/ does, would be nice if ruff started supporting dead code rules however!

---

_Comment by @ntBre on 2025-03-04 03:37_

Thanks for opening the issue! I think this is being tracked in #872. As mentioned there, this will require the multi-file analysis and type inference that we're working on in red-knot.

---

_Closed by @ntBre on 2025-03-04 03:37_

---
