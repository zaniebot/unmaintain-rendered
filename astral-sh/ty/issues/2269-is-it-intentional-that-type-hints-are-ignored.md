```yaml
number: 2269
title: Is it intentional that type hints are ignored when using setattr?
type: issue
state: closed
author: phi-friday
labels:
  - question
assignees: []
created_at: 2025-12-30T05:10:15Z
updated_at: 2025-12-30T07:58:06Z
url: https://github.com/astral-sh/ty/issues/2269
synced_at: 2026-01-12T15:54:26Z
```

# Is it intentional that type hints are ignored when using setattr?

---

_@phi-friday_

### Question

```python
class A:
    a: int
    def __setattr__(self, key, value):
        ...

a = A()
a.a = "test"
```
https://play.ty.dev/fd8418ae-991f-43ee-b9f0-2f513d77efdb

In the code above, a is an int, so I expected an error to be raised when assigning "test", but it didn't occur.
It seems to be due to setatt, is this intentional or a bug?

### Version

playground b925ae506

---

_Label `question` added by @phi-friday on 2025-12-30 05:10_

---

_Renamed from "s it intentional that type hints are ignored when using setattr?" to "Is it intentional that type hints are ignored when using setattr?" by @phi-friday on 2025-12-30 05:10_

---

_Comment by @phi-friday on 2025-12-30 05:13_

Is this related to issue #1460?

---

_Comment by @MichaReiser on 2025-12-30 07:58_

Yes, I think you're right. This is https://github.com/astral-sh/ty/issues/1460#issuecomment-3474720694. Adding type annotations to value in `__setattr__` makes ty raise an error on the assignment.

---

_Closed by @MichaReiser on 2025-12-30 07:58_

---
