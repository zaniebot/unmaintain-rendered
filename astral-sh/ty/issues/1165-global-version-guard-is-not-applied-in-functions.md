```yaml
number: 1165
title: Global version guard is not applied in functions
type: issue
state: open
author: hynek
labels:
  - unreachable-code
assignees: []
created_at: 2025-09-10T16:13:53Z
updated_at: 2026-01-08T18:34:43Z
url: https://github.com/astral-sh/ty/issues/1165
synced_at: 2026-01-10T01:56:40Z
```

# Global version guard is not applied in functions

---

_Issue opened by @hynek on 2025-09-10 16:13_

### Summary

I tried adding ty to _svcs_ in https://github.com/hynek/svcs/pull/141 since it's heavy on typing stuff and I've encountered this: https://play.ty.dev/004edf8e-54d0-4a7c-897b-78ebcfa2cdf8

Basically I want to define a function depending on the Python version and use different APIs. If I pull the version_info check into the function, it passes.

```py
import sys, inspect
from collections.abc import Callable


if sys.version_info >= (3, 10):

    def _robust_signature(factory: Callable) -> inspect.Signature | None:
        try:
            # Provide the locals so that `eval_str` will work even if the user
            # places the `Container` under a `if TYPE_CHECKING` block.
            sig = inspect.signature(
                factory,
                locals={"Container": object()},
                eval_str=True,
            )
        except Exception:  # noqa: BLE001
            # Retry without `eval_str` since if the annotation is "svcs.Container"
            # the eval will fail due to it not finding the `svcs` module
            try:
                sig = inspect.signature(factory)
            except Exception:  # noqa: BLE001
                return None

        return sig
else:

    def _robust_signature(factory: Callable) -> inspect.Signature | None:
        try:
            return inspect.signature(factory)
        except Exception:  # noqa: BLE001
            return None

```

### Version

ty 0.0.1-alpha.20 (f41f00af1 2025-09-03)

---

_Label `unreachable-code` added by @carljm on 2025-09-10 16:16_

---

_Added to milestone `GA` by @carljm on 2025-09-10 16:16_

---

_Comment by @carljm on 2025-09-10 16:16_

Thanks for the report! Yeah, it looks like we should probably skip checking any scope whose definition isn't reachable.

---
