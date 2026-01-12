```yaml
number: 708
title: Ty does not support conditional imports
type: issue
state: open
author: Matt-Ord
labels:
  - imports
  - typing semantics
assignees: []
created_at: 2025-06-26T09:10:53Z
updated_at: 2025-11-14T15:24:04Z
url: https://github.com/astral-sh/ty/issues/708
synced_at: 2026-01-12T15:54:23Z
```

# Ty does not support conditional imports

---

_@Matt-Ord_

### Summary

Loving the progress on ty 
Just thought I would flag this issue I was having when type checking conditional imports, which fails on Implicit shadowing of class `SymmetricalLogScale`. Is there another way do this that would be allowed under ty rules?

```py
from __future__ import annotations

from typing import TYPE_CHECKING, Literal

import numpy as np

try:
    from matplotlib.scale import LinearScale, LogScale, SymmetricalLogScale

except ImportError:
    # Implicit shadowing of class `SymmetricalLogScale` here
    LinearScale, LogScale, SymmetricalLogScale = (None, None, None)


if TYPE_CHECKING:
    from matplotlib.scale import ScaleBase


Scale = Literal["symlog", "linear", "log"]


def get_scale_with_lim(
    scale: Scale,
    lim: tuple[float, float],
) -> ScaleBase:
    if LinearScale is None or LogScale is None or SymmetricalLogScale is None:
        msg = "Matplotlib is not installed. Please install it with the 'plot' extra."
        raise ImportError(msg)
    match scale:
        case "linear":
            return LinearScale(axis=None)
        case "symlog":
            max_abs = max([np.abs(lim[0]), np.abs(lim[1])])
            return SymmetricalLogScale(
                axis=None,
                linthresh=1 if max_abs <= 0 else 1e-3 * max_abs,
            )
        case "log":
            max_abs = max([np.abs(lim[0]), np.abs(lim[1])])
            return LogScale(axis=None)
    # Also does not infer that this is unreachable
```

---

_Comment by @sharkdp on 2025-06-26 09:26_

Thank you for reporting this.

We treat an import as both a declaration and a binding. So if `SymmetricalLogScale` is successfully imported, it's declared as `<class 'SymmetricalLogScale'>`. Since we (currently) do not statically determine if that `except ImportError` branch will be executed or not, we also see the `SymmetricalLogScale = None` binding and that leads to the error.

I'm not sure if that's the best idea here, but one way that you could make this work would be to guard the `… = (None, None, None)` binding with `if not TYPE_CHECKING:`. This way, `SymmetricalLogScale` will either have type `<class 'SymmetricalLogScale'>` (if `matplotlib` is available), or type `Unknown`  (if `matplotlib` is not available). `Unknown` is a dynamic type that should not cause any downstream type errors.

Playground example: https://play.ty.dev/86568c8a-0c3e-4d51-acbd-22b28a1608d1 (try removing the `optional_library.py` tab. The only thing that will change is the inferred type for `SomeClass`)


--

I think it's plausible that we could improve our understanding here for simple `try: …; except ImportError: …` patterns. In that case, we could mark the `try` and the `except` branch as mutually exclusive (only one of them is statically reachable, depending on the success of the import). The type of the imported symbol would then either be the imported type or `None`, and no diagnostic would be emitted.

---

_Comment by @Matt-Ord on 2025-06-26 09:31_

The issue with using it not Type checking is that I want to infer class symmetricLogScale | None so I can correctly check for none inside get scale with lim

---

_Comment by @carljm on 2025-06-26 16:40_

The simplest solution here would be to stop erroring on incompatible declarations, and just consider it our semantics in such cases that we union the declared types.

---

_Label `imports` added by @AlexWaygood on 2025-10-06 16:14_

---

_Label `typing semantics` added by @AlexWaygood on 2025-10-06 16:15_

---

_Comment by @carljm on 2025-11-14 15:24_

Although it looks a bit silly, the easiest workaround for this today is to change this:

```py
try:
    from module import C
except ImportError:
    C = None
```

to this:

```py
try:
    from module import C
except ImportError:
    C: None = None
```

If the assignment of `None` is also "declared", then ty is fine with the two declarations and will union type to `C | None`, as desired.

I still think that we should consider doing something different in ty here, though.

---

_Added to milestone `Stable` by @carljm on 2025-11-14 15:24_

---
