---
number: 7540
title: "Linter: Add warning when attempting to set module attributes"
type: issue
state: open
author: tomasalagoa
labels:
  - rule
  - type-inference
assignees: []
created_at: 2023-09-20T11:14:47Z
updated_at: 2024-10-24T14:33:42Z
url: https://github.com/astral-sh/ruff/issues/7540
synced_at: 2026-01-10T01:22:47Z
---

# Linter: Add warning when attempting to set module attributes

---

_Issue opened by @tomasalagoa on 2023-09-20 11:14_

As mentioned in https://lwn.net/Articles/943619/ some developers had an issue distinguishing between a module object and a context object from the module

```python
import numpy as np
import mpmath as mp  # should be from mpmath import mp
mp.dps = 50
```

This led to an attempt to change the behavior of how Python handled `__setattr__` for module objects but was rejected due to poor performance, though I believe it could be a linter rule in ruff.

I've tried to do this but cannot figure out how to get a list of imported modules at runtime, though maybe the rule could be directed only towards the `mp` module alias.

---

_Comment by @charliermarsh on 2023-09-20 13:24_

Am I correct that looking at the import statement itself is insufficient, and instead, we'd need to be able to determine whether `mp` in `from mpmath import mp` refers to a module or an object? (Since `mp` could be a module _within_ `mpmath`.)

---

_Label `rule` added by @charliermarsh on 2023-09-20 13:24_

---

_Label `waiting-on-author` added by @charliermarsh on 2023-09-20 13:24_

---

_Comment by @tomasalagoa on 2023-09-20 14:27_

> Am I correct that looking at the import statement itself is insufficient, and instead, we'd need to be able to determine whether `mp` in `from mpmath import mp` refers to a module or an object? (Since `mp` could be a module _within_ `mpmath`.)

I believe so, as the issue here is the developer intended to set the `dps` attribute of the `mp` object in `mpmath`, not create a `dps` object inside the `mpmath` module.

---

_Label `waiting-on-author` removed by @charliermarsh on 2023-09-20 20:58_

---

_Label `type-inference` added by @charliermarsh on 2023-09-20 20:58_

---

_Label `needs-decision` added by @charliermarsh on 2023-09-20 20:58_

---

_Label `multifile-analysis` added by @charliermarsh on 2023-09-20 20:58_

---

_Label `needs-decision` removed by @charliermarsh on 2023-09-20 20:58_

---

_Label `type-inference` removed by @charliermarsh on 2023-09-20 20:58_

---

_Comment by @charliermarsh on 2023-09-20 20:58_

We're probably blocked on being able to implement this for now, since we need to be able to resolve `mp` to either a module or an object.

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:33_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:33_

---
