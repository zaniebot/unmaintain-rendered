---
number: 5284
title: When packages are unavailable due to wheel tag Python requirement mismatches, error message should reflect that
type: issue
state: closed
author: charliermarsh
labels:
  - error messages
assignees: []
created_at: 2024-07-22T13:21:25Z
updated_at: 2024-07-22T18:31:09Z
url: https://github.com/astral-sh/uv/issues/5284
synced_at: 2026-01-07T13:12:17-06:00
---

# When packages are unavailable due to wheel tag Python requirement mismatches, error message should reflect that

---

_Issue opened by @charliermarsh on 2024-07-22 13:21_

E.g., given `echo "dearpygui==1.11.1" | cargo run pip compile --universal --python 3.12.3 --verbose -`, I think we're rejecting the package because there are no wheels with the Python tags we need (though it might be a buggy, but that's separate), yet the message says:

```
  × No solution found when resolving dependencies:
  ╰─▶ Because the requested Python version (>=3.12.3) does not satisfy Python>=3.12.3 and dearpygui==1.11.1 depends on Python>=3.12.3, we can conclude that dearpygui==1.11.1 cannot be used.
      And because you require dearpygui==1.11.1, we can conclude that the requirements are unsatisfiable.

      hint: The `Requires-Python` requirement (>=3.12.3) includes Python versions that are not supported by your dependencies (e.g., dearpygui==1.11.1 only supports >=3.12.3). Consider using a more restrictive `Requires-Python`
      requirement (like >=3.12.3).
```

---

_Label `error messages` added by @charliermarsh on 2024-07-22 13:21_

---

_Referenced in [astral-sh/rye#1259](../../astral-sh/rye/issues/1259.md) on 2024-07-22 13:45_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-22 13:45_

---

_Referenced in [astral-sh/uv#5296](../../astral-sh/uv/pulls/5296.md) on 2024-07-22 17:56_

---

_Closed by @charliermarsh on 2024-07-22 18:31_

---
