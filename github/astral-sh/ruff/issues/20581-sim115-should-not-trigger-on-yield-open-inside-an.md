---
number: 20581
title: "SIM115 should not trigger on `yield open(...)` inside an @contextmanager function"
type: issue
state: open
author: zackw
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-09-25T18:32:38Z
updated_at: 2025-09-25T19:48:37Z
url: https://github.com/astral-sh/ruff/issues/20581
synced_at: 2026-01-07T13:12:16-06:00
---

# SIM115 should not trigger on `yield open(...)` inside an @contextmanager function

---

_Issue opened by @zackw on 2025-09-25 18:32_

### Summary

This function trips SIM115:

```
import os
from contextlib import contextmanager

@contextmanager
def wrapped_open(path):
    fd = os.open(path, os.O_RDWR | os.O_CREAT | os.O_EXCL)
    try:
        yield open(fd, "w+b", closefd=False)  # SIM115 on this line
        os.fsync(fd)
    except:
        os.unlink(path)
        raise
    finally:
        os.close(fd)
```
(https://play.ruff.rs/da1c8437-ac81-46a9-89cd-0ae0179d4b14)

IMO this is a false positive; use of `with` in this function would complicate rather than simplify the logic.

I suggest suppressing SIM115 when the `open` call is the argument of a `yield` statement and the function is decorated with `contextlib.contextmanager` (not necessarily spelled exactly thus).

(Related to, but not the same as, #8221)

---

_Label `rule` added by @ntBre on 2025-09-25 19:36_

---

_Label `needs-decision` added by @ntBre on 2025-09-25 19:36_

---

_Comment by @ntBre on 2025-09-25 19:48_

This seems like a fairly rare use case. It might be better to use a noqa in this scenario than try to add a more general exception to the rule, but I'm open to other thoughts!

On the other hand, we do already have an exception for `return` (https://github.com/astral-sh/ruff/issues/13862):

https://github.com/astral-sh/ruff/blob/e4ac9e90419841a338d6909af01acc8548c8b3c3/crates/ruff_linter/src/rules/flake8_simplify/rules/open_file_with_context_handler.rs#L244-L247

so it might be reasonable to extend that to `yield`, irrespective of the context manager part.

---
