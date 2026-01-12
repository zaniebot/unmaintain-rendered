```yaml
number: 5858
title: PERF203 should only trigger for exception handler that unconditionally raises
type: issue
state: closed
author: NeilGirdhar
labels:
  - needs-info
assignees: []
created_at: 2023-07-18T13:27:05Z
updated_at: 2023-07-28T04:55:57Z
url: https://github.com/astral-sh/ruff/issues/5858
synced_at: 2026-01-12T15:54:45Z
```

# PERF203 should only trigger for exception handler that unconditionally raises

---

_@NeilGirdhar_

I think that PERF203 should only trigger when the exception handler unconditionally raises.
```python
for i in range(10):
    try:
        print(1 / 0)
    except Exception:  # a.py:4:5: PERF203 `try`-`except` within a loop incurs performance overhead
        print("oops")
    print("2")
```
Although, this is not a priority for me since I decided to disable it.  It never caught anything useful for me.

---

_Label `waiting-on-author` added by @charliermarsh on 2023-07-19 01:22_

---

_Comment by @charliermarsh on 2023-07-19 01:22_

For completeness -- could you give an example of a case that should and shouldn't be caught, based on this criteria?

---

_Comment by @NeilGirdhar on 2023-07-19 03:24_

Where it should not be caught is, in my opinion constructs like the one displayed above.  The problem is that this error can't be acted on for such an example.  You can't move the try/except outside the loop since that would make an exception skip everything that follows the try/except.

As for where it should be caught, I think it's tricky because there's a balance.  While it is more efficient to have try/except around the loop, it means that more code is protected by the try/except, which is a downside.  Ideally, you want the minimum amount of code in a try/except.

I'm not sure what others like, but personally I don't find this error useful in the first place since it is trying to add more code to the try/except.

---

_Closed by @charliermarsh on 2023-07-28 04:55_

---
