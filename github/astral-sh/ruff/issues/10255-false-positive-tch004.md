---
number: 10255
title: False positive TCH004
type: issue
state: open
author: tlambert03
labels:
  - rule
assignees: []
created_at: 2024-03-06T19:24:33Z
updated_at: 2024-03-11T19:48:06Z
url: https://github.com/astral-sh/ruff/issues/10255
synced_at: 2026-01-07T13:12:15-06:00
---

# False positive TCH004

---

_Issue opened by @tlambert03 on 2024-03-06 19:24_

I occasionally use the following pattern in an `__init__` when I want to publicly re-export a name (`"RiskyExport"`) from a sub-module (`.dangerous_submodule`), without greedily importing that submodule (for example, if it requires an additional module-level dependency that may or may not be installed)

```python
from typing import TYPE_CHECKING, Any

__all__ = ["RiskyExport"]

if TYPE_CHECKING:
    from .dangerous_submodule import RiskyExport

def __getattr__(name: str) -> Any:
    if name == "RiskyExport":
        from .dangerous_submodule import RiskyExport  # here's the actual import

        return RiskyExport
    raise AttributeError(f"module {__name__!r} has no attribute {name!r}")
```

ruff will complain with:

- Move import `.dangerous_submodule. RiskyExport` out of type-checking block. Import is used for more than type hinting. Ruff(TCH004)

but it's *not* used for more than type hinting at the top level, because i import it again inside of `__getattr__` if someone explicitly imports that name.

Note that if I remove `RiskyExport` from `__all__`, then the error changes to F401 (imported by unused)

---

_Comment by @MichaReiser on 2024-03-07 08:00_

That does make sense to me. To summarize what I understand you're suggesting: The idea is to change the heuristic to not flag `RiskyExport` as runtime type because it gets re-imported as runtime type in all paths referencing the runtime value. 

I don't know if we have the capabilities to do this today, because it requires that the analysis is path-sensitive (it doesn't just track whether a symbol is imported and used as typing or runtime type, but doing so for each possible path through your program)

---

_Label `rule` added by @MichaReiser on 2024-03-07 08:01_

---

_Comment by @tlambert03 on 2024-03-11 19:48_

yep, your summarized understanding is correct.  and I agree it is path sensitive, you'd need to check whether the name being used for more than type hinting wasn't available in either its global *or* local namespace

---
