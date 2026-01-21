```yaml
number: 2580
title: "Methods on `Protocol` classes with trivial bodies should be understood as implicitly abstract"
type: issue
state: open
author: AlexWaygood
labels: []
assignees: []
created_at: 2026-01-21T16:18:11Z
updated_at: 2026-01-21T16:24:06Z
url: https://github.com/astral-sh/ty/issues/2580
synced_at: 2026-01-21T17:03:34Z
```

# Methods on `Protocol` classes with trivial bodies should be understood as implicitly abstract

---

_@AlexWaygood_

This is a followup to https://github.com/astral-sh/ruff/pull/22753. Ty should emit `abstract-method-in-final-class` on `G` here, but currently does not:

```py
from typing import Protocol, final

class F(Protocol):
    def f(self) -> int: ...

@final
class G(F): ...
```

The reason why is that all methods on `Protocol` classes with an explicit non-`None` return type and a trivial body should be understood as "implicitly abstract". Ty exempts such methods from the rule that would cause us to complain about this method saying it returns `int` when it actually returns `None`; that exemption is only sound if we therefore understand the method as being implicitly abstract. We should extract the `is_stub_suite` function here into a location where it can be reused, and then reuse it in our determination of which methods are abstract on a class if that class is a `Protocol` class: https://github.com/astral-sh/ruff/blob/22f240dadf58837f9ac55e5f06d591449f620847/crates/ty_python_semantic/src/types/infer/builder.rs#L2404-L2445. 

Cc. @charliermarsh

---

_Added to milestone `Stable` by @AlexWaygood on 2026-01-21 16:24_

---
