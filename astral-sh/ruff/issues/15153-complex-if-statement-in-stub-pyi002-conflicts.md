---
number: 15153
title: "`complex-if-statement-in-stub (PYI002)` conflicts with `if-with-same-arms (SIM114)`"
type: issue
state: open
author: Avasam
labels:
  - rule
assignees: []
created_at: 2024-12-26T21:32:13Z
updated_at: 2024-12-28T04:58:21Z
url: https://github.com/astral-sh/ruff/issues/15153
synced_at: 2026-01-10T01:22:56Z
---

# `complex-if-statement-in-stub (PYI002)` conflicts with `if-with-same-arms (SIM114)`

---

_Issue opened by @Avasam on 2024-12-26 21:32_

In https://github.com/python/typeshed/pull/13309 I found that the following rules can be in conflict:
- [complex-if-statement-in-stub (PYI002)](https://docs.astral.sh/ruff/rules/complex-if-statement-in-stub/#complex-if-statement-in-stub-pyi002)
- [if-with-same-arms (SIM114)](https://docs.astral.sh/ruff/rules/if-with-same-arms/#if-with-same-arms-sim114)

Example:
```py
import sys

if sys.version_info >= (3, 11):  # SIM114
    OP_IGNORE_UNEXPECTED_EOF: int
elif sys.version_info >= (3, 8) and sys.platform == "linux":
    OP_IGNORE_UNEXPECTED_EOF: int
```

```py
import sys

if sys.version_info >= (3, 11) or (sys.version_info >= (3, 8) and sys.platform == "linux"):  # PYI002
    OP_IGNORE_UNEXPECTED_EOF: int
```

What would the recommendation be here? Could either rule be adapted so they work together ?

In this specific case, granted that typeshed's oldest supported Python version is 3.8, this could be adapted to:
```py
import sys

if sys.version_info >= (3, 11) or sys.platform == "linux":
    OP_IGNORE_UNEXPECTED_EOF: int
```

But the underlying issue still exists.

---

Ruff 0.8.4

---

_Referenced in [python/typeshed#13309](../../python/typeshed/pulls/13309.md) on 2024-12-26 21:34_

---

_Comment by @MichaReiser on 2024-12-27 17:23_

Thanks for opening this issue. 

I see how the interaction here is unfortunate but I also worry that simplifying the boolean condition would require a SAT solver or similar, which seems a bit overkill, but I might be wrong on this. 

While unfortunate, I do think that the current interaction between the two rules is reasonable:

* It honors the user's intention to flag and fix identical-if-arms
* It leaves it to the user to simplify the if condition OR to disable `SIM114` if the condition can't be simplified. 



---

_Label `rule` added by @MichaReiser on 2024-12-27 17:23_

---

_Comment by @Avasam on 2024-12-28 04:58_

If the official recommendation is there's no better solution than to noqa and it's up to the dev to decide what they prefer. Then that answers my question. I wasn't sure if maybe I was missing something.

---
