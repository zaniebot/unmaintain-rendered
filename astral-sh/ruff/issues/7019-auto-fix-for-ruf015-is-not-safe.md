---
number: 7019
title: Auto fix for RUF015 is not safe
type: issue
state: closed
author: jaap3
labels: []
assignees: []
created_at: 2023-08-31T12:59:02Z
updated_at: 2023-08-31T13:09:43Z
url: https://github.com/astral-sh/ruff/issues/7019
synced_at: 2026-01-10T01:22:46Z
---

# Auto fix for RUF015 is not safe

---

_Issue opened by @jaap3 on 2023-08-31 12:59_

Using `ruff 0.0.286` the following:

```python
bar = (n for n in ())
try:
    foo = list(bar)[0]
except IndexError:
    pass
else:
    print(foo)
```

is rewritten to:

```python
bar = (n for n in ())
try:
    foo = next(iter(bar))
except IndexError:
    pass
else:
    print(foo)
```

The rewritten code is now prone to raising an uncaught `StopIteration` error.

Same thing goes for the rewrite of:

```python
import contextlib

bar = (n for n in ())

with contextlib.suppress(IndexError):
    foo = list(bar)[0]
    print(foo)
```

---

_Comment by @charliermarsh on 2023-08-31 13:01_

Thanks — this is mentioned in the documentation (https://beta.ruff.rs/docs/rules/unnecessary-iterable-allocation-for-first-element/) and the rule is marked internally as a suggested fix. In the future, suggested fixes will require a dedicated flag to run rather than running automatically, which should help with this.

---

_Closed by @charliermarsh on 2023-08-31 13:01_

---

_Comment by @jaap3 on 2023-08-31 13:08_

👍 thanks, didn't think to check the docs for this. I will mark it as unfixable in our config for now (with a reference to this ticket). Looking forward to a version that doesn't apply this fix by default, any idea how far in the future that might be? Can I watch an issue/PR to stay updated?

---

_Comment by @MichaReiser on 2023-08-31 13:09_

You can track the applicability work here https://github.com/astral-sh/ruff/issues/4185 (or the parent issue)

---

_Referenced in [astral-sh/ruff#7769](../../astral-sh/ruff/pulls/7769.md) on 2023-10-03 17:27_

---
