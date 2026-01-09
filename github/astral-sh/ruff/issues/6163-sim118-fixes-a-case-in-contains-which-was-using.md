---
number: 6163
title: SIM118 fixes a case in __contains__ which was using self, causing recusrion
type: issue
state: closed
author: HexDecimal
labels:
  - bug
assignees: []
created_at: 2023-07-28T23:46:26Z
updated_at: 2023-07-29T03:35:28Z
url: https://github.com/astral-sh/ruff/issues/6163
synced_at: 2026-01-07T13:12:15-06:00
---

# SIM118 fixes a case in __contains__ which was using self, causing recusrion

---

_Issue opened by @HexDecimal on 2023-07-28 23:46_

An issue similar to #4481, Ruff incorrectly marks a case of [SIM118](https://beta.ruff.rs/docs/rules/in-dict-keys/) as an error and as fixable when neither is the case.

```py
from typing import KeysView


class Foo:
    def keys() -> KeysView[object]:
        ...

    def __contains__(self, key: object) -> bool:
        return key in self.keys()  # SIM118 replaces this with `return key in self`
```
The above is a likely pattern when writing a subclass of `collections.abc.Mapping`.

Command: `ruff example.py --select SIM --fix`
Version: `ruff 0.0.280`


---

_Comment by @charliermarsh on 2023-07-29 00:02_

We can make the rule ignore self.keys which will almost always be wrong, but that’s probably the best we can do at present.

---

_Comment by @charliermarsh on 2023-07-29 00:04_

Or ignoring this rule in __contains__ would also be reasonable.

---

_Comment by @HexDecimal on 2023-07-29 00:08_

I'm not in a hurry. I'm already ignoring these manually and checked to see if there was a issue for this.

Ignoring `self.keys()` in `__contains__` might work.  Since this issue is very specific.

---

_Label `bug` added by @charliermarsh on 2023-07-29 00:27_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-29 03:25_

---

_Referenced in [astral-sh/ruff#6165](../../astral-sh/ruff/pulls/6165.md) on 2023-07-29 03:27_

---

_Closed by @charliermarsh on 2023-07-29 03:35_

---
