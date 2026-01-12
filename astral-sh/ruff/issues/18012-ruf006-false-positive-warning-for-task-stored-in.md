```yaml
number: 18012
title: "RUF006: False positive warning for task stored in global variable"
type: issue
state: open
author: 5j9
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-05-11T02:57:44Z
updated_at: 2025-05-12T08:14:38Z
url: https://github.com/astral-sh/ruff/issues/18012
synced_at: 2026-01-12T15:54:56Z
```

# RUF006: False positive warning for task stored in global variable

---

_@5j9_

### Summary

Consider the following sample code:
```python
from asyncio import new_event_loop, sleep

loop = new_event_loop()
task = loop.create_task(sleep(1))
loop.run_until_complete(sleep(2))
```

Despite `task` being a global variable, Ruff reports the following error:

```bash
$ ruff check --isolated \~scratch.py --select RUF006
~scratch.py:4:8: RUF006 Store a reference to the return value of `loop.create_task`
  |
3 | loop = new_event_loop()
4 | task = loop.create_task(sleep(1))
  |        ^^^^^^^^^^^^^^^^^^^^^^^^^^ RUF006
5 | loop.run_until_complete(sleep(2))
  |

Found 1 error.
```
**Expected Behavior**
Since `task` is global, storing a reference in it should not be problematic. Ruff should not flag this code.

### Version

ruff 0.11.9

---

_Label `rule` added by @MichaReiser on 2025-05-12 07:28_

---

_Label `needs-decision` added by @MichaReiser on 2025-05-12 07:28_

---

_Comment by @MichaReiser on 2025-05-12 07:28_

I think that change would be okay, considering that modules stick around forever (usually). However, this might also be a situation where it's fine to use a `noqa`.

---

_Comment by @5j9 on 2025-05-12 08:08_

Note that this situation is similar to issue #9262, in my opinion. Both involve storing the task in a global variable; the only difference is that, in the other case, the global declaration occurs within the function.

---
