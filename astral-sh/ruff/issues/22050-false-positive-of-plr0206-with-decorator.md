```yaml
number: 22050
title: False-positive of PLR0206 with decorator injecting parameter into method
type: issue
state: open
author: fritz-k
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-12-18T10:41:29Z
updated_at: 2025-12-18T14:24:59Z
url: https://github.com/astral-sh/ruff/issues/22050
synced_at: 2026-01-12T15:54:58Z
```

# False-positive of PLR0206 with decorator injecting parameter into method

---

_@fritz-k_

### Summary

When injecting parameters from decorators, ruff complains about additional parameters on a property (PLR0206):

The sample code ([playground](https://play.ruff.rs/d56db4a0-9311-4c0e-9445-b791d2c61c4e))
```python
from __future__ import annotations

from functools import wraps
from typing import TYPE_CHECKING, Concatenate, get_args

if TYPE_CHECKING:
    from collections.abc import Callable


def inject_arg[I, T, **P](
    fn: Callable[Concatenate[I, str, P], T],
) -> Callable[Concatenate[I, P], T]:

    @wraps(fn)
    def wrapped(instance: I, *args: P.args, **kwargs: P.kwargs) -> T:
        additional_param = "some_arg"
        return fn(instance, additional_param, *args, **kwargs)

    return wrapped


class Test:
    @property
    @inject_arg
    def prop(
        self,
        additional_param: str,
    ) -> str:
        return additional_param
```
produces
```
    Cannot have defined parameters for properties (PLR0206) [Ln 25, Col 9]
```

Exempting the offending parameter
```diff
-        additional_param: str,
+        additional_param: str,  # noqa: PLR0206  - Additional param is injected -> false-positive
```
doesn't silence the finding, only
```diff
-    def prop(
+    def prop(  # noqa: PLR0206  - Additional param is injected -> false-positive
```
does. With the entire method being exempt, the (later) addition of non-injected parameters is not caught by ruff.

Ideally ruff can detect the decorator injecting arguments, but if that is not feasible, allowing the offending parameters to be exempted individually would still prove useful.

---

_Comment by @ntBre on 2025-12-18 14:24_

Thanks for the report!

I think it would be quite difficult to understand the decorator to the extent that it would suppress the rule. But I kind of agree about the noqa comment positioning. From a quick local test, we could accept your first noqa placement if we change the diagnostic range from the method name to the parameter itself. We would have to be careful because that's a breaking change, though.

---

_Label `rule` added by @ntBre on 2025-12-18 14:24_

---

_Label `needs-decision` added by @ntBre on 2025-12-18 14:24_

---
