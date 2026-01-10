```yaml
number: 740
title: "Support `type()`'s functional syntax"
type: issue
state: open
author: InSyncWithFoo
labels:
  - runtime semantics
assignees: []
created_at: 2025-07-01T13:31:02Z
updated_at: 2025-12-27T04:16:00Z
url: https://github.com/astral-sh/ty/issues/740
synced_at: 2026-01-10T01:56:40Z
```

# Support `type()`'s functional syntax

---

_Issue opened by @InSyncWithFoo on 2025-07-01 13:31_

(First raised [on Discord](https://discord.com/channels/1039017663004942429/1279201882337705994/1389547211150331974).)

Currently ty doesn't understand `type()`'s functional syntax very well (playground):

```python
# error: Object of type `type` is not assignable to `type[int]`
U32: type[int] = type('U32', (int,), {})
```

It understands `type(...)` returns a new type, but fails to take the bases into account.

---

_Label `runtime semantics` added by @carljm on 2025-07-02 17:17_

---

_Added to milestone `Stable` by @carljm on 2025-11-12 23:55_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-26 22:51_

---

_Unassigned @charliermarsh by @charliermarsh on 2025-12-26 23:21_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-27 00:16_

---

_Comment by @condemil on 2025-12-27 04:16_

Another example:

```python
from unittest import TestCase

tests: list[type[TestCase]] = []
testCaseClass = type("Foo", (TestCase,), {})

# Argument to bound method `append` is incorrect: Expected `type[TestCase]`, found `type`
tests.append(testCaseClass)
```

---
